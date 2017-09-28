<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

# Bouncing Box

We're going to create a simple game where a box moves across the screen at an increased speed after each click.

[When you are done it should look like this](http://jsbin.com/toyikozeyi/1)

Our goal for this game is to learn how to bring together HTML, CSS, and JavaScript. We use HTML to define our structure, CSS to define the style of that structure, and JavaScript in order to implement behavior. One of the primary ways we can implement behavior in JavaScript is by making modifications to the HTML and CSS in response to **events** which we will demonstrate by making this simple game. 

## Let's get started
* Make sure your github and cloud9 accounts are linked to Greenlight
* Open your first website workspace
* go to your bash terminal (located at the bottom of the cloud9 workspace) and enter the command **os install**
* use your arrow keys to scroll to bouncing-box and hit enter
* type in Y and hit enter
* you will be editing the index.html file

NOTE 1: If you receive an error that says os install command not found
* enter the command ``` npm install -g opspark ``` and try again

NOTE 2: If you are asked to enter your credentials
* Type in your github username and hit enter
* Type in Y and hit enter
* Type in your password and hit enter. It won't look like you're typing but it's just hiding your text. It may ask you again and give an error but after that os install will work

## A note about jQuery

We are going to be using [jQuery](https://jquery.com) for this exercise. You can see that we've included it in our web page with the following HTML 

    <script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>

jQuery is a powerful library which makes building web pages easier. It is also tremendously popular. If you are doing web development in 2015, you will likely run into jQuery. That is why we are introducing just a tiny bit of it here. 

You can recognize jQuery by its use of a very curious function `$()` Here is some of the jQuery code we use in this page:

    box = $('.box');
    boardWidth = $('.board').width();

### TODO 1: Create and Style Box

The HTML for our box has already been created for us:

    <body class="board">
      <div class="box">?</div>
    </body>

Find the CSS that styles the box and change the following properties:

- `left` (how many pixels from the left of the screen the box is - **as this increases the box moves right**)
- `top` (how many pixels from the top of the screen the box is - as this increases the box moves down)
- `width` (how wide the box is pixels)
- `height` (how tall the box is in pixels)

Notice how you can change the appearance of the box using CSS! Now return those to their original values

    width: 70px;
    height: 70px;
    top: 100px;
    left: 0px;

### TODO 2: Learn how to move the box using the box.css() function

We discovered earlier that **the `left` css property changes how far from the left side of screen the box is**. To make our box move we want to change the `left` css property. The `box.css()` function lets us change css properties using JavaScript. 

Add the following code below `TODO 2`

    //TODO 2
    var position;
    
    position = 0;

    box.css('left', position);

`box.css('left', position);` sets the value of the `left` css property to the value held in our `position` variable which is currently 0. Increase the value of the `position` variable and watch the box move to the right!

Before we move on, lets reset position to 0 so it moves back to the left side of the screen.

    position = 0;  


Declare and initialize `points` variables to zero and `speed` to 10 by adding the following under `TODO 2`.

    var position;
    var points;
    var speed;

    position = 0;
    points = 0;
    speed = 10;

JavaScript lets us change HTML as well. Add the following JavaScript code under `TODO 2`

    box.text(points);

Change the `points` variable and watch how it changes the box. 



### TODO 3: Animating the box - Think of a flip book!

You can create animation on a web page by changing the appearance of an object over time. A traditional animation is made up of individual "frames" of still images. If you flip between these images rapidly and each image is just slightly different than the previous image, the viewer sees the scene as motion. We do the same thing in programming. 

The `setInterval` function allows us to setup a timer, where we call a function every so often. **How often**, the time between function calls, is called the interval. That interval is expressed in milliseconds, or thousandths of a second. 

The following code calls our `update` function every 1000 milliseconds or every second. 
(It's already there so you don't need to copy/paste this):
    
    setInterval(update, 1000);

Now we want our update function to animate the box. Add the following code to the update function.

    update = function() {
        position = position + speed;
        box.css('left', position);
    };

Every time the `update` function is called (or every second) the position variable is increased and the box is set to that new position. 

Change the second argument in setInterval() to 50 to it looks like this now:

    setInterval(update, 50)

Now the update function is being called every 50 milliseconds (or 20 times a second) making the box move faster and more smoothly across the screen.

### TODO 4: Hey box, come back!

Each time we call the `update` function the position variable gets larger and larger until eventually our box has gone off the screen. The position of our box should never be greater than the width of the board. Add the following code **inside** the `update` function

    if(position > boardWidth) {
        position = 0;
    }

Your entire `update` function should look like this:

    update = function() {
        position = position + speed;
        if(position > boardWidth) {
         position = 0;
        }
        box.css('left', position);
    };

### TODO 5: Handling events

An event is just a particular thing that has happened. Some examples of **events** are:

- A timer going off
- The user clicking on something
- the web page has finished loading

JavaScript allows us to change the web page in response to **events**. The following code calls the `handleBoxClick` function every time the box is clicked. (It's already there so you do not need to copy/paste this)

    box.on('click', handleBoxClick);

Every time the user clicks the box, we want to reset the box to its starting position and make the game harder by increasing the speed of the box. Add the following code to the `handleBoxClick` function

    handleBoxClick = function() {
      speed = speed + 3;
      position = 0;
    }

### TODO 6: Keeping Score

We want to keep track of how many times the user has clicked on the box. 

Add the following code to the `handleBoxClick` function

     points = points + 1;

and then add the following code to the `update` function

    box.text(points);

What's going on here?

### TODO 7: Make It Bounce

So we have the box loop accross the screen, but don't we want it to bounce off of the walls?
Well, at least we want to make it look that way.

Before we can make it bounce we have to figure out how to make the box move from right to left.
Right now our motion comes from the following line in the `update` function:

    position = position + speed;
    
Since `speed` is positive, `position` keeps getting bigger, so to make the box 
move the other way we need to subtract the speed instead of adding it.
To do this we can add a variable `direction` that will tell us 
whether to add or subract the speed. Let's declare and initialize it up top:

    var direction;
    direction = 1;
    
Now change the `update` function to set the value of position like so:

    position = position + (speed * direction);
    
When `direction` is set to 1 then this increases the position by `speed`,
sending the box to the right.  But when `direction` is set to -1,
the speed is subracted from the position, sending the box to the left.

So, in our update function we have this if statement to make sure our box doesn't go off the right side of the screen. 

    if(position > boardWidth) {
        position = 0;
    }
We need to change this bounds-check so that instead of resetting the position of our box to 0 we change the direction to -1.
Do this and confirm that the box bounces off the right wall.

Now that your box bounces off the right wall you'll need to make it bounce off the left wall. What should the bounds check be to make this happen? Do this yourself!

####Hint: At what position value do you want the box to "bounce" off the left wall?

## Good Job

You've written your first game! Here are some ways you can try and make your game even more awesome.

### Can you move the box up and down?
### Can you change the size or color of the box with each click?
### Use the [background-image](http://www.w3schools.com/cssref/pr_background-image.asp) CSS property to change your box or the background

