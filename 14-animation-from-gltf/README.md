# Using the gltfLoader

A format which is getting more and more attention lately is the glTF (GL Transmission) format. This format, for which you can find a very extensive explanation at https://github.com/KhronosGroup/glTF, focuses on optimizing size and resource usage.<br>

The working code for the example can be found in the file named `14-animation-from-gltf.html`<br>

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/14-animation-from-gltf.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/14-animation-from-gltf.html"><h3>Preview</h3></a>

## Concepts
Using the `glTFLoader` is similar to the other loaders. **Note** that besides the `THREE.GLTFLoader`, there is also a `THREE.LegacyGLTFLoader`. The first one should normally be used, since it supports glTF Versions 2.0 and up (which is the current standard). If you have older versions, then you can use the `THREE.LegacyGLTFLoader`. To use this loader, include the correct JavaScript file<br>
```js
  <script type="text/javascript" charset="UTF-8" src="../../libs/three/loaders/GLTFLoader.js"></script>
```

Use the `THREE.GLTFLoader` as follows<br>
```js
  var loader = new THREE.GLTFLoader();
  
  loader.load('../../assets/models/CesiumMan/CesiumMan.gltf', function (result) {
    // correctly position the scene
    result.scene.scale.set(6, 6, 6);
    result.scene.translateY(-3);
    result.scene.rotateY(-0.3*Math.PI);
    scene.add(result.scene);
    
    // setup the mixer
    mixer = new THREE.AnimationMixer( result.scene );
    animationClip = result.animations[0];
    clipAction = mixer.clipAction( animationClip ).play();
    animationClip = clipAction.getClip();
    
    // add the animation controls
    enableControls();
  });
```

This loader also loads a complete scene, so you can either add everything in the group, or select child elements.
