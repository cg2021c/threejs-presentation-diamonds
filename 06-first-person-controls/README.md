# First Person Controls

As the name implies, FirstPersonControls allows you to control the camera just
like in a first-person shooter. The mouse is used to look around, and the
keyboard is used to walk around.

Creating these controls follows the same principles as the ones followed for
other controls we’ve seen until now. The example we've made uses the
following configuration:

```js
var fpControls = new THREE.FirstPersonControls(camera);
fpControls.lookSpeed = 0.4;
fpControls.movementSpeed = 20;
fpControls.lookVertical = true;
fpControls.constrainVertical = true;
fpControls.verticalMin = 1.0;
fpControls.verticalMax = 2.0;
fpControls.lon = -150;
fpControls.lat = 120;
```

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/06-first-person-camera.html"><h3>Code</h3></a>

<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/06-first-person-camera.html"><h3>Preview</h3></a>

## Usage

The controls for First Person Controls are as follows:

|Control|Action|
|-------|------|
|Arrows|Move left, right, forward, or backward|
|Mouse movement|Look around|
|W|Move forward|
|S|Move backward|
|A|Move left|
|D|Move right|
|R|Move up|
|F|Move down|
|Q|Stop movement|

## Properties

There are a couple of properties that you can use to fine-tune how the camera acts. Here are some of their properties and what is controls:

|Property|Result|
|--------|------|
|Property|Result|
|--------|------|
|.enabled|Whether controls are enabled|
|.autoForward|Whether or not the camera is automatically moved forward|
|.constrainVertical|Whether or not looking around is vertically constrained by [.verticalMin, .verticalMax]|
|.verticalMax|How far you can vertically look around, upper limit. Range is 0 to Math.PI radians|
|.verticalMin|How far you can vertically look around, lower limit.|
|.activeLook|Whether or not it's possible to look around|
|.lookVertical|Whether or not it's possible to vertically look around|
|.lookSpeed|The look around speed|
|.movementSpeed|The movement speed|
