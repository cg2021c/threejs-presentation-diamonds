# Animation Loaded from a Quake model

The MD2 format was created to model characters from Quake, a great game from 1996. Even though the newer engines use a different format, you can still find a lot of interesting models in the MD2 format. Using an MD2 file works the same as any of the others we’ve seen so far.<br>

The working code for the example can be found in the file named `13-animation-from-md2.html`

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/13-animation-from-md2.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/13-animation-from-md2.html"><h3>Preview</h3></a>

## Concepts
When loading an MD2 model, you get a geometry, so you have to make sure that you create a material first and assign a skin.<br>

```js
  var textureLoader = new THREE.TextureLoader();
  var loader = new THREE.MD2Loader();
  
  loader.load('../../assets/models/ogre/ogro.md2', function (result) {
    var mat = new THREE.MeshStandardMaterial(
    { morphTargets: true,
      color: 0xffffff,
      metalness: 0,
      map: textureLoader.load('../../assets/models/ogre/skins/skin.jpg')
    })
    
    var mat2 = new THREE.MeshNormalMaterial();
    var mesh = new THREE.Mesh(result, mat);
    scene.add(mesh);
    
    //setup the mixer
    mixer = new THREE.AnimationMixer(mesh);
    animationClip1 = result.animations[7];
    clipAction1 = mixer.clipAction( animationClip1 ).play();
    animationClip2 = result.animations[9];
    clipAction2 = mixer.clipAction( animationClip2 );
    animationClip3 = result.animations[10];
    clipAction3 = mixer.clipAction( animationClip3 );
    
    // add the animation controls
    enableControls(result);
});
```

In the preceding code, you can see that we use a `THREE.MeshStandardMaterial` for the skin of the model. We also create three animations based on the information from the loaded model. Since we don’t have bones in this model, we have to set the `morphTargets` property to `true` to see the animations.<br>

