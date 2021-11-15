# Loading an Animation from COLLADA Model

COLLADA is an XML-based schema that enables you to transfer data among 3D digital content creation tools, such as SketchUp, Maya, 3ds Max, and Rhino. COLLADA files use the `.dae` file extension, which stands for digital asset exchange.<br>

The working code for the example can be found in the file named `12-animation-from-collada.html`

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/12-animation-from-collada.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/12-animation-from-collada.html"><h3>Preview</h3></a>

## Concepts
Loading a model from a COLLADA file works in the same manner as for the other formats. First, you have to include the correct loader JavaScript file.


```js
  <script type="text/javascript" src="../libs/ColladaLoader.js"></script>
```

While the normal COLLADA models can get quite large, there is also a `KMZLoader` available in Three.js. This is basically a compressed COLLADA model, so if you run into Keyhole Markup language Zipped (KMZ) models, you can follow the steps here, and use the `KMZLoader` instead of the `ColladaLoader`.<br>

```js
  var loader = new THREE.ColladaLoader();
  loader.load('../../assets/models/monster/monster.dae', function (result) {
    scene.add(result.scene);
    result.scene.rotateZ(-0.2*Math.PI)
    result.scene.translateX(-20)
    result.scene.translateY(-20)
    // setup the mixer
    mixer = new THREE.AnimationMixer(result.scene);
    animationClip = result.animations[0];
    clipAction = mixer.clipAction( animationClip ).play();
    animationClip = clipAction.getClip();
     // add the animation controls
    enableControls();
  });
```

A COLLADA file can contain multiple mode such as storing complete scenes, including cameras, lights, animations, and more. A good way to work with a COLLADA model is to print out the result from the `loader.load()` function to the console and determine which components you want to use.<br>

In Three.js, we can add `THREE.Group` elements to a `THREE.Scene`. If the COLLADA file contains elements that want to be excluded, you can just select the ones from the scene property to add. To render and animate this model, all we have to do is set up the animation just as we did for the Blender-based model.
