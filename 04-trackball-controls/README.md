# Trackball Controls

Before you can use TrackballControls, you first need to include the correct JavaScript file in your page:

```html
<script type="text/javascript" src="../libs/TrackballControls.js"></script>
```

With this included, we can create the controls and attach them to the
camera, as follows:

```js
var trackballControls = new THREE.TrackballControls(camera);
trackballControls.rotateSpeed = 1.0;
trackballControls.zoomSpeed = 1.0;
trackballControls.panSpeed = 1.0;
```

Updating the position of the camera is something we do in the render loop, as follows:

```js
var clock = new THREE.Clock();
function render() {
var delta = clock.getDelta();
trackballControls.update(delta);
requestAnimationFrame(render);
webGLRenderer.render(scene, camera);
}
```

In the preceding code snippet, we see a new Three.js object, **THREE.Clock**. 
The **THREE.Clock** object can be used to exactly calculate the elapsed time that a
specific invocation or rendering loop takes to complete. You can do this by
calling the **clock.getDelta()** function. This function will return the elapsed time
between this call and the previous call to **getDelta()**. To update the position of
the camera, we call the **trackballControls.update()** function. In this function, we
need to provide the time that has passed since the last time this update
function was called. For this, we use the **getDelta()** function from
the T**HREE.Clock** object. You might wonder why we don’t just pass in the frame
rate (1/60 seconds) to the update function. The reason is that
with **requestAnimationFrame**, we can expect 60 fps, but this isn’t guaranteed.
Depending on all kinds of external factors, the frame rate might change. To
make sure the camera turns and rotates smoothly, we need to pass in the exact
elapsed time.

A working example for this can be found in 04-trackball-controls-camera.html.
The following links show a code and a preview of this example:

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/04-trackball-controls-camera.html"><h3>Code</h3></a>

<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/04-trackball-controls-camera.html"><h3>Preview</h3></a>

## Usage

The controls for Trackball Controls are as follows:

|Control|Action|
|-------|------|
|Left mouse button and move|Rotate the camera around the scene|
|Scroll wheel|Zoom in and out|
|Middle mouse button and move|Zoom in and out|
|Right mouse button and move|Pan the camera around the scene|

## Properties

There are a couple of properties that you can use to fine-tune how the camera acts. Here are some of their properties and what is controls:

|Property|Result|
|--------|------|
|.enabled|Whether controls are enabled|
|.rotateSpeed|How fast the camera rotates|
|.panSpeed|How fast the camera pans|
|.rotateSpeed|How fast the camera rotates|
|.maxDistance|How far you can zoom out|
|.minDistance|How far you can zoom in|
|.mouseButtons|This object contains references to the mouse actions used by the controls.<ul><li>.LEFT is assigned with THREE.MOUSE.ROTATE</li><li>.MIDDLE is assigned with THREE.MOUSE.ZOOM</li><li>.RIGHT is assigned with THREE.MOUSE.PAN</li></ul>|
|.noPan|Whether or not panning is disabled|
|.noRotate|Whether or not rotation is disabled|
|.noZoom|Whether or not zooming is disabled|
