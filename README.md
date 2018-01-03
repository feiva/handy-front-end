# handy-front-end
Handy help scripts for a front-end developer


### included:

Some handy scripts for a easier experience building the web:

- [Canvas.js](#canvasjs)
- [Pointer.js](#pointerjs)

## Canvas.js
Makes it much easier handling the awesome canvas

Script link:

[https://gustavgenberg.github.io/handy-front-end/Canvas.js](https://gustavgenberg.github.io/handy-front-end/Canvas.js)

```javascript
// Initialize
const canvas = new Canvas( id [ width, height, context ] );
```
```javascript
// Drawing individual frames
canvas.drawFrame(function ( rendererContext, handyObject ) {
	
	rendererContext.fillText('Hello', 10, 10);
	
});

// Can also be reset and cleared easely for drawing new frames:
canvas.reset();
canvas.clear( [ bool preserveTransform ] );

// Important note!
// When the canvas element is resized, it will automatically clear its content!
// Therefore the single frame drawing is not suitable for apps where the canvas element is resized (often fullscreen)!

```
```javascript
// Drawing speed adapted to clients capabilities
// Catch the `draw` event. Functions is given arguments described in the Events section below.
canvas.on( 'draw', function ( rendererContext, handyObject, delta ) {

	// rendererContext : provided rendererContext ( default CanvasRenderingContext2D )
	// handyObject : object that is available in all event callback functions. very handy!
	// delta

	rendererContext.fillText('Hello', 10, 10);
	
	// in every event callback, the `this` keyword is referring to the instance

});
```

Events:

`draw` => rendererContext, handyObject, delta

`update` => handyObject, delta

`start` => rendererContext, handyObject

`stop` => rendererContext, handyObject

`pause` => rendererContext, handyObject

`resume` => rendererContext, handyObject

```javascript
// Functions

// Start rendering
canvas.start(); // const canvas = new Canvas(...).start() also works but then the `start` event is fired right ahead and is impossible to catch afterhand

// Stop rendering
canvas.stop();

// Pause update loop
canvas.pause();

// Resume update loop
canvas.resume();


// Get base64 image of canvas
canvas.toImage( [ format ] ); // format default is png

```
```javascript
//functions added to window.CanvasRenderingContext2D:

clear(); // clears the whole canvas
		// equal to clearRect(0, 0, width, height);
reset(); // resets the transform made on canvas
		// equal to setTransform(1, 0, 0, 1, 0, 0);
```
```javascript
// Full example

// Initialize
const canvas = new Canvas('my-canvas', 500, 500);

// Catch `start` event
canvas.on('start', function (renderingContext, handyObject) {

	// set start position
	handyObject.x = 5;

});

// Catch `update` event aka update loop, logic loop, game loop (clientside)
canvas.on('update', function (handyObject, delta) {

	// Use delta to move object, making sure object moves equally fast no matter the framerate (except 0 heh.. no moving at all)
	handyObject.x += 100 * delta;

});

// Catch the `draw` loop
canvas.on('draw', function (rendererContext, handyObject, delta) {

	// Clear the whole canvas
	rendererContext.clear();

	// Draw moving rectangle
	rendererContext.fillRect(handyObject.x, 10, 10, 10);

});

canvas.start();
```

## Pointer.js
Script that keeps tracking the mouse pointer easy.

Script link:

[https://gustavgenberg.github.io/handy-front-end/Pointer.js](https://gustavgenberg.github.io/handy-front-end/Pointer.js)

```javascript
// Initialize
const pointer = new Pointer( [ element (default window) ] );

// Get mouse x
pointer.x
// Get mouse y
pointer.y

// returns undefined if not known


// Get mouse pointer path ( elements that the pointer is pointing at)
pointer.path
// Get the "closest" element (like p)
pointer.path[0]
// Get "faraway" element (like body, html)
pointer.path[.. > 0]

// returns undefined if not known


// Get the state of a button
pointer.isDown(pointer.BTN_LEFT); // true or false

// Buttons:
// pointer.BTN_LEFT
// pointer.BTN_MIDDLE
// pointer.BTN_RIGHT

// Disable the right-click context menu
pointer.disableContextMenu();

// Enable the right click context menu that was disabled by this script!
pointer.enableContextMenu();

// Get newest clickHistory entry
pointer.getLastClick(); // => {t, x, y, endx, endy} t = timestamp x,y is the mousedown pos and endx,endy is the mouseup pos.

// add event listener
pointer.on( 'mousedown'/'mouseup'/'mousemove' , function (event) {
	// event => {t: timestamp, x: x-pos, y: y-pos, evt: raw event}
});

// oncontextmenu of Pointer.element
pointer.on( 'contextmenu', function (raw event) {

});


new Pointer() =>

// variables

BTN_LEFT: 0
BTN_MIDDLE: 1
BTN_RIGHT: 2
buttonsDown: []
clickHistory: []
element: [default window]
listeners: Map(0) {}
mousedown: {t: null, x: null, y: null, evt: null}
mouseup: {t: null, x: null, y: null, evt: null}
moveHistory: [] // last 1000 mousemoves
path: null // last mousemove's event.path
preventContextMenu: ƒunction
x: null
y: null

//functions

disableContextMenu: ƒ disableContextMenu()
enableContextMenu: ƒ enableContextMenu()
getLastClick: ƒ getLastClick()
isDown: ƒ isDown(button)
on: ƒ on(event,fn)
relative: ƒ relative()

// used by Pointer
// bindElementListeners: ƒ bindElementListeners()
// fireEvent: ƒ fireEvent(event)

```
