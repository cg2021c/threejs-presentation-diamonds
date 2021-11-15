# Creating Bones Animation using Blender

 Start with an animation created in Blender and export the file using the Three.js exporter. When exporting the file using the Three.js exporter, make sure that the following 
properties are checked to export the animation as a skeletal animation instead of a morph animation. With a skeletal animation, the movements of the bones are exported, which we 
can replay in Three.js, as follows:

```js
var loader = new THREE.JSONLoader();
loader.load('../../assets/models/hand/hand-8.json', function (result) {
  var mesh = new THREE.SkinnedMesh(result, new THREE.MeshNormalMaterial({skinning: true}));
  mesh.scale.set(18, 18, 18)
  scene.add(mesh);
  // setup the mixer
  mixer = new THREE.AnimationMixer( mesh );
  animationClip = mesh.geometry.animations[0];
  clipAction = mixer.clipAction( animationClip ).play();
  animationClip = clipAction.getClip();
});

function render() {
  ..
  var delta = clock.getDelta();
  mixer.update( delta );
  ..
}
```

To run this animation, we have to create a `THREE.SkinnedMesh` and, use a `THREE.AnimationMixer`, a `THREE.AnimationClip`, and a `THREE.ClipAction` together to tell Three.js how to play the exported animation. Also don't forget to call the `mixer.update()` function in the render loop as well.<br>

The following links show a code and a preview of this example:

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/11-animation-from-blender.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/11-animation-from-blender.html"><h3>Preview</h3></a>

## Important things on creating animation in Blender:<br>
- Every vertex from the model must at least be assigned to a vertex group.<br>
- The name of the vertex groups used in Blender must correspond to the name of the bone that controls it. That way, Three.js can determine which vertices it needs to modify when moving the bones.<br>
- Only the first action is exported. So, make sure the animation you want to export is the first one.<br>
- When creating keyframes, it is a good idea to select all the bones even if they don’t change.<br>
- When exporting the model, make sure the model is in its rest pose and is selected. If this is not the case, there will be a very deformed animation.<br>
- It is preferred to just export a single object together with its animation, instead of exporting the complete scene.<br><br>
