Canvas is powerful api
- draw images, shapes, text, patterns
- <canvas id="c" width="200" height = "200"></canvas>
- grab canvas context for drawing stuff

```
var c = document.querySelector("#c");
var ctx = c.getContext("2d"); // can also be 3d, but 2d for now.

```
- Directly saving modified cnavas images are not possbile(.toDataURL()) until,
- the images are stored on a local or online server. till then right click save image.

use `ctx.fillRect(50,50,50,50)` and `ctx.strokeRect(50,50,50,50)` to fill rect with black color or jsut have border with black.


use `fillStyle` for background color. use `clearRect` to change some strokes or clear canvas. shortcut is `c.width=c.width`

beginPath, moveTo, lineTo,fill and stroke to draw graphs.

Checkout Scaling,  Rotation,Translation in this order of operation.

ctx.save() and ctx.restore() to save and restore canvas states.

ctx.fillStyle and strokeStyle should be applied before the elements are drawn to be applied.


checkout https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Manipulating_video_using_canvas

and http://www.tannerhelland.com/3643/grayscale-image-algorithm-vb6/

### Pixels to animaitons
request animation frame for applying filters to videos or fast changing content
Game loop
- a sequence of events that runs contionusouly till app or game is being used.
- use kibo library, for processing inputs as upp, down, numbers instead of keycodes.
- if processing  mouse inputs, remeber to subtract offsetleft and top to get canvas specific values. (By default they are browser speicifc)
```
var c = document.querySelector("canvas");

function handleMouseClick(evt) {
        x = evt.clientX - c.offsetLeft;
        y = evt.clientY - c.offsetTop;
        console.log("x,y:"+x+","+y);
}
c.addEventListener("click", handleMouseClick, false);
```

Checkout http://www.html5gamedevelopment.com/html5-game-tutorials/2013-12-developing-html5-games-1hr-video-presentation and /r/gamedev
