# Visualize motions captured models using the fbxLoader
The FBX format is used to provide interoperability between digital content creation applications and game engines such as Blender, Maya, Autodesk, Unity, Unreal and many others. It supports many features such as 3D models, scene hierarchy, materials, lighting, animations, bones and more. There are many resource that provide you to find animations, for example https://www.mixamo.com/.
The working code can be found in the file named `15-animation-from-fbx.html`.

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/15-animation-from-fbx.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/15-animation-from-fbx.html"><h3>Preview</h3></a>

## Concepts
```js

var loader = new THREE.FBXLoader();
  loader.load('../../assets/models/salsa/salsa.fbx', function (result) {
    result.scale.set(0.2, 0.2, 0.2);
    result.translateY(-13);
    scene.add(result)
    
    // setup the mixer
    mixer = new THREE.AnimationMixer( result );
    animationClip = result.animations[0];
    clipAction = mixer.clipAction( animationClip ).play(); 
    animationClip = clipAction.getClip();
    enableControls();
  })
```
