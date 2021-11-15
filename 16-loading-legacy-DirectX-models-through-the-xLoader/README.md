# Loading legacy DirectX models through the xLoader
Microsoft's DirectX used to have a specific format (in old versions) to store 3D models and animations. Though it is hard to find models in this format, with the THREE.XLoader, we can use the models in Three.js. An example on how to do this can be found in `16-animation-from-x.html`.

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/16-animation-from-x.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/16-animation-from-x.html"><h3>Preview</h3></a>

## Concepts
To work with the THREE.XLoader, we first use a THREE.XLoader to load the model, then a THREE.XLoader to load the animation, and finally, we have to call assignAnimation to connect the animation to the loaded model. Once that is done, we can follow the standard way of animating the model.
```js

var manager = new THREE.LoadingManager();
var textureLoader = new THREE.TextureLoader();
var loader = new THREE.XLoader( manager, textureLoader );
var animLoader = new THREE.XLoader( manager, textureLoader );
// we could also queue this or use promises
loader.load(["../../assets/models/x/SSR06_model.x"], function (result) {
  var mesh = result.models[0];
  animLoader.load(["../../assets/models/x/stand.x", { putPos: false, putScl: false }], function (anim) { 
    animLoader.assignAnimation(mesh);
    // at this point we've got a normal mesh, and can get the mixer and clipaction
    mixer = mesh.animationMixer;
    clipAction = mixer.clipAction( "stand" ).play();
    var clip = clipAction.getClip();
    mesh.translateY(-6)
    mesh.rotateY(-0.7*Math.PI);
    scene.add(mesh)
  });
});
```
