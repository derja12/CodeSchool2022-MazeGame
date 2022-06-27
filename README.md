# CodeSchool2022-MazeGame

Instructions on creating a Maze-like game using Vue and css attributes.

Working version hosted at https://derja12.github.io/Maze-Game/

## Instructions

1. <a href="https://github.com/derja12/CodeSchool2022-MazeGame/edit/main/README.md#1-display-the-player-green-square-in-the-example">displaying the player</a>
2. <a href="https://github.com/derja12/CodeSchool2022-MazeGame/edit/main/README.md#2-add-movement-to-the-player">adding movement</a>
3. <a href="https://github.com/derja12/CodeSchool2022-MazeGame/edit/main/README.md#3-add-static-walls">Add static walls</a>
4. <a href="https://github.com/derja12/CodeSchool2022-MazeGame/edit/main/README.md#4-add-collision-detection">Add collision detection</a>
5. Add ctrl+click placing down walls
6. Add a backend database which you can export/import arrays of walls (in other words, _maze levels_)
7. Add a level browser which a user can pull different levels from a list provided by the backend

<br><br>
### 1. display the player (green square in the example)

&emsp; &emsp; If you have *one object* that represents **the player's style**:

```js
data {
  player_style: {
    position: 'absolute',
    width: '50px', 
    height: '50px', 
    bottom: '50px',
    left: '50px'
    // ...
  }
}
```

&emsp;&emsp; you can then bind that object to the player div's style. This would look like: 

```html
<div id="player" v-bind:style="player_style"></style>
```

&emsp;&emsp; Don't forget to add something that you can see to the styling (background color, outline, etc.). Otherwise, it'll be hard to tell if your player is showing up.

<br><br>
### 2. Add movement to the player

&emsp; &emsp; This will require you editing the 'left' and 'bottom' during specific _events_ (ie. user presses a wasd keys)

&emsp; &emsp; **To get the events working**, you'll want to add an event listener on the _window_:

```js
methods: {
  keyDownEvents: function (event) {
    console.log(event); // look at what your console logs - that'll tell you how to get the key presed
    // if w
      this.moveUp();
    // if a
      this.moveLeft();
    // ...
  },
  moveUp: function () {
    // increase player_style.bottom
  },
  moveLeft: function () {
    // decrease player_style.left
  },
  // ...
},
created: function () {
  window.addEventListener("keydown", this.keyDownEvents);
}
```

&emsp; &emsp; When you increase/decrease the position of the player, remember that the value for 'left' and 'bottom' is the string _"50px"_. This means you need to parse the number by getting rid of the px at the end, then add/subtract the movement value. After, you'll want to update the object with the _new value + "px"_:

```js
// MOVE PLAYER UP

// parse the current bottom value
let old_bottom = Number(this.player_style.bottom.slice(0, -2));
// move up by 50 pixels 
let new_bottom = old_bottom + 50; 
// update the player_style.bottom
this.player_style.bottom = String(new_bottom) + "px";
```

&emsp; &emsp; Get each movement method to trigger only when their respective key is pressed. I recommend centralizing some of the code by creating helper functions (ex: create a method to update the player bottom, or retrieve the current _value_ of the player's bottom). If you need to change/update this code later on, you only need to update those functions rather than each individual time you parse/update the value.

<br><br>
### 3. Add static Walls

&emsp; &emsp; To get some walls to show up, you'll want to have an array that holds wall objects. Each wall object will be very similar to the player object, describing it's postion, size, etc.

```js
data: {
  player_style: {...},
  
  walls: [
    // Wall 5px wide, 100px tall, located at (350px, 200px)
    {
      position: "absolute",
      width: "5px",
      height: "100px",
      left: "350px",
      bottom: "200px"
    },
    // Wall 100px wide, 5px tall, located at (350px, 200px)
    {
      position: "absolute",
      width: "100px",
      height: "5px",
      left: "350px",
      bottom: "200px"
    },
    // ...
  ]
}
```

&emsp; &emsp; It can be helpful to write a method that converts two opposite-corner points ((x1, y1), (x2, y2)) into one of these objects. (If you want to implement placing down walls later with ctrl+click, I highly recommend getting this to work at this point).

```js
// NOTE: this example function is not a method inside the vue instance, but just a helper regular javascript function
function: convertWallCoordinatesToObject(x1, y1, x2, y2) {
  // calculate the width, height, and location
  
  // create a new object with those values
  
  // return the wall object
}
```

&emsp; &esmp; Now we can push a new wall by simply supplying bottom left and top right coordinates to the function, pushing on the returned value:

```js
created: function () {
  // when the vue instance is created, create these 5 walls
  this.walls.push(convertWallCoordinateToObject(400, 405, 100, 400));
  this.walls.push(convertWallCoordinateToObject(795, 800, 100, 400));
  this.walls.push(convertWallCoordinateToObject(400, 800, 100, 105));
  this.walls.push(convertWallCoordinateToObject(400, 510, 395, 400));
  this.walls.push(convertWallCoordinateToObject(590, 800, 395, 400));
}
```

&emsp; &emsp; You'll notice that the player can still move through the walls, and for now, _that's okay_. We don't have anything preventing that from happening yet, which brings us to the next step.

<br><br>
### 4. Add Collision Detection

_Coming Soon._
