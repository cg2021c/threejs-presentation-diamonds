# Fly Controls

The next control we’ll look at is FlyControls. With FlyControls, you can fly around a scene using controls also found in flight simulators.

Enabling FlyControls works in the same manner as TrackballControls. First, load the correct JavaScript file:
```html
<script type="text/javascript" src="../libs/FlyControls.js"></script>
```

Next, we configure the controls and attach them to the camera, as follows:
```js
var flyControls = new THREE.FlyControls(camera);
flyControls.movementSpeed = 25;
flyControls.domElement = document.querySelector('#WebGL-output');
flyControls.rollSpeed = Math.PI / 24;
flyControls.autoForward = true;
flyControls.dragToLook = false;
```

Next, the property that needs to be set correctly is the domElement property. This property should point to the
element in which we render the scene. For our example, we used the following element for our output:

```html
<div id="webgl-output"></div>
```

We set the property like this:

```js
flyControls.domElement = document.querySelector('#webgl-output');
```

If we don’t set this property correctly, moving the mouse around will result in
strange behavior. 

The following links show a code and a preview of this example:

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/05-fly-controls-camera.html"><h3>Code</h3></a>

<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/05-fly-controls-camera.html"><h3>Preview</h3></a>

# Controls

You can control the camera with THREE.FlyControls in the
following manner:

|Control|Action|
|-------|------|
|Left and middle mouse button|Move forward|
|Right mouse button|Move backward|
|Mouse movement |Look around|
|W|Move forward|
|S|Move backward|
|A|Move left|
|D|Move right|
|R|Move up|
|F|Move up|
|Arrows|Look left, right, up, or down|
|G|Roll left|
|E|Roll right|


There are a couple of properties that you can use to fine-tune how the camera acts. Here are some of their properties and what is controls:

|Property|Result|
|--------|------|
|.autoForward|If set to true, the camera automatically moves forward (and does not stop) when initially translated|
|.dragToLook|If set to true, you can only look around by performing a drag interaction|
|.movementSpeed|The movement speed|
|.rollSpeed|The rotation speed|
