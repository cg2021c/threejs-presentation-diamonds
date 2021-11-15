# Orbit Controls

The **OrbitControl** control is a great way to rotate and pan around an object in the
center of the scene.

Using OrbitControl is just as simple as using the other controls. Include the
correct JavaScript file, set up the control with the camera, and
use THREE.Clock again to update the control:

```js
<script type="text/javascript" src="../libs/OrbitControls.js"></script>
...
var orbitControls = new THREE.OrbitControls(camera);
orbitControls.autoRotate = true;
var clock = new THREE.Clock();
...
var delta = clock.getDelta();
orbitControls.update(delta);
```

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/07-controls-orbit.html"><h3>Code</h3></a>

<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/07-controls-orbit.html"><h3>Preview</h3></a>

## Controls

The controls for THREE.OrbitControls are focused on using the mouse, as shown
in the following table:

|Control|Action|
|-------|------|
|Left mouse click + move|Rotate the camera around the center of the scene|
|Scroll wheel|Zoom in and out|
|Middle mouse button and move|Zoom in and out|
|Right mouse button and move|Pan the camera around the scene|
|Arrows|Pan the camera around the scene|

## Properties

There are a couple of properties that you can use to fine-tune how the camera acts. Here are some of their properties and what is controls:

|Property|Result|
|--------|------|
|.autoRotate|Set to true to automatically rotate around the target. Note that if this is enabled, you must call .update () in your animation loop|
|.enabled|When set to false, the controls will not respond to user input.|
|.enablePan|Enable or disable camera panning.|
|.enableRotate|Enable or disable horizontal and vertical rotation of the camera.|
|.enableZoom|Enable or disable zooming (dollying) of the camera|
|.minDistance|How far you can zoom in|
|.panSpeed|Speed of panning|

These are just a snippet of the properties you can tweak for OrbitControls. To see more, don't forget to check the <a href="https://threejs.org/docs/?q=orbit#examples/en/controls/OrbitControls">ThreeJS documentation
for OrbitControls</a>.

## Conclusion of Cameras

That’s it for the camera and moving it around. In this part, we’ve seen a lot of
controls that allow you to easily interact with and move through the scene by
changing the camera properties. In the next section, we’ll look at more
advanced methods of animation: morphing and skinning.
