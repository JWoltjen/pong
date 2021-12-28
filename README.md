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

## How does the game handle a lose condition? 
