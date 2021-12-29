# pong
a simple pong clone done in vanilla js. 


## What are getters and setters?

Getters and setters are called accessor properties. They were introduced to JS in 2009 (ES5). They execute on getting and setting values. They are used by both the Ball and Paddle class to get and set their respective positions each time the script updates. The position of the Ball and paddle objects is originally set in Styles.css. Then the getter functions run:

         parseFloat(getComputedStyle(this.ballElem).getPropertyValue("--x"))

to change the calculated CSS value into a float value that can be used by JS to assign a value to the position of the elmement. Because the paddles can only go up or down, they only have getters and setters for the y axis, but the ball needs getters and setters for both the y and x axis to calculate movement across the screen. 

## How does the computer paddle follow the ball? 

Paddle has an update method as part of its class which takes in two parameters: the time since the last update (delta) and the value of the ball's y position (ballheight). The update function then changes the paddle's computed position (this.position) based on the speed variable, how long the game has lasted, and how far away the ball's height is from the paddle's computed position: 
        
        update(delta, ballHeight) {
        this.position += SPEED * delta * (ballHeight - this.position)
        }
        
This is then called in scripts JS inside the update loop of scripts, such that every time the ball's position moves, the computer paddle tries to make its position identical (though it is constrained by a global SPEED variable, meaning that if the ball accelerates too quickly, the AI will not be able to keep up): 
        
        computerPaddle.update(delta, ball.y)


## How does the game keep track of time? 

The key is the window.requestAnimationFrame, which takes in the function update() as its parameter: 

       window.requestAnimationFrame(update)
       
 The update function itself aims to determine how many frames have passed since the last time the window.requestAnimationFrame(update) function ran. If it's the first time the function has run, then lastTime is assigned to time, and the function calls itself again, creating a loop. The loop is broken if a lose condition is reached. Otherwise, if it is not the first time the function has run, it will find the delta by taking: const delta = time - lastTime. 
 
The beauty of this function is that no matter how much time it takes for a computer to update the screen, the other moving objects can calibrate their movement based on whatever the value of delta is. 
 
    let lastTime 
    function update(time) {
    if (lastTime != null){
    const delta = time - lastTime
    ball.update(delta, [playerPaddle.rect(), computerPaddle.rect()])
    computerPaddle.update(delta, ball.y)

        if(isLose()){
           handleLose()
        }
    }
    lastTime = time
    window.requestAnimationFrame(update)
         }

## How does the ball update its movement? 


## How does the game handle a lose condition? 

You guessed it, it must be part of the update function--meaning that the program checks if the game is lost with every update. The lose condition in pong is if the ball passes the paddle and gets to the edge of the respective player's screen. First, it declares rect, and sets it to the ball's rect() prototype method. The isLose() method simply returns whether rect.right or rect.left is out of bounds of the screen's innerwidth value or less than zero. 
.rect() is an important prototype method for games, because it delivers positional value, x,y, height, width relative to the window the element is being displayed in. 
