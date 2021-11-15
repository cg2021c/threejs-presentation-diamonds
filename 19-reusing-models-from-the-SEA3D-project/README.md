# Reusing models from the SEA3D project
SEA3D is an open source project that can be used to create games, make
models, add animations, and much more.  For now, this is how you can use the models from this project 
and visualize them in Three.js. An example of this is shown in `18-animation-from-sea.html`.

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/18-animation-from-sea.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/18-animation-from-sea.html"><h3>Preview</h3></a>

## Concepts
The main difference, as you can see from the code, is that we have provided
the `THREE.Scene` that we want to models loaded into when we create the loader
(which is called `THREE.SEA3D`). Something else that is different compared to the
other loaders is that instead of specifying the callback in the load call, we have
to specify the callback through the onComplete function. Once the scene has been
loaded, though, we process it just as we do for other animations.
```js

var sceneContainer = new THREE.Scene();
var loader = new THREE.SEA3D({
  container: sceneContainer
});
loader.load('../../assets/models/mascot/mascot.sea');
loader.onComplete = function( e ) {
  var skinnedMesh = sceneContainer.children[0];
  skinnedMesh.scale.set(0.1, 0.1, 0.1);
  skinnedMesh.translateX(-40);
  skinnedMesh.translateY(-20);
  skinnedMesh.rotateY(-0.2*Math.PI);
  scene.add(skinnedMesh);
  // and set up the animation
  mixer = new THREE.AnimationMixer( skinnedMesh );
  animationClip = skinnedMesh.animations[0].clip;
  clipAction = mixer.clipAction( animationClip ).play(); 
  animationClip = clipAction.getClip();
  enableControls();
};
  
```
