# Bones Manually

Morph animations are very straightforward. Three.js knows all the target vertex positions and only needs to transition each vertex from one position to the next. For bones and skinning, it becomes a bit more complex. When you use bones for animation, you move the bone, and Three.js has to determine how to translate the attached skin (a set of vertices) accordingly. The example is in the link below :

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/10-bones-manually.html"><h3>Code</h3></a>

<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/10-bones-manually.html"><h3>Preview</h3></a>

For this example, we use a model that was exported from Blender to the Three.js format (hand-1.js in the models/hand folder). This is a model of a hand, complete with a set of bones. By moving the bones around, we can animate the complete model. Let’s first look at how we loaded the model:

```
loader.load('../../assets/models/hand/hand-1.js', function (geometry, mat) {
  var mat = new THREE.MeshLambertMaterial({color: 0xF0C8C9, skinning: true});
  mesh = new THREE.SkinnedMesh(geometry, mat);
  mesh.scale.set(15,15,15);
  mesh.position.x = -5;
  mesh.rotateX(0.5*Math.PI);
  mesh.rotateZ(0.3*Math.PI);
  scene.add(mesh);
  startAnimation();
});
```

Loading a model for bone animation isn’t that different than any of the other models. We just specify the model file, which contains the definition of vertices, faces, and also bones, and, based on that geometry, we create a mesh. Three.js provides a specific mesh for skinned geometries such as these, called **THREE.SkinnedMesh**. 

The one thing you need to specify to make sure the model is updated is set the skinning property of the material you use to true. If you don’t set this to true, you won’t see any bone movement. In this example, we’ll use a tween object to handle the animation, which is created in the **startAnimation()** function. This tween instance is defined as follows:

```
function startAnimation() {
  tween = new TWEEN.Tween({pos: -1.5})
  .to({pos: 0}, 3000)
  .easing(TWEEN.Easing.Cubic.InOut)
  .yoyo(true)
  .repeat(Infinity)
  .onUpdate(onUpdate);
  tween.start();
}
```

With this tween, we transition the pos variable from -1.5 to 0. We’ve also setthe yoyo property to true, which causes our animation to run in reverse the nexttime it is run, and, to make sure our animation keeps running, we setrepeat to Infinity. You can also see that we specify an onUpdate method. This method is used to position the individual bones, and we’ll look at that next.

When you open the example preview in the link that mentioned earlier, you see the hand making a grab-like motion. We did this by setting the z rotation of the finger bones in the onUpdate method that is called from our tween animation, as follows:

```
var onUpdate = function () {
var pos = this.pos;
// rotate the fingers
mesh.skeleton.bones[5].rotation.set(0, 0, pos);
mesh.skeleton.bones[6].rotation.set(0, 0, pos);
mesh.skeleton.bones[10].rotation.set(0, 0, pos);
mesh.skeleton.bones[11].rotation.set(0, 0, pos);
mesh.skeleton.bones[15].rotation.set(0, 0, pos);
mesh.skeleton.bones[16].rotation.set(0, 0, pos);
mesh.skeleton.bones[20].rotation.set(0, 0, pos);
mesh.skeleton.bones[21].rotation.set(0, 0, pos);
// rotate the wrist
mesh.skeleton.bones[1].rotation.set(pos, 0, 0);
};
```

Whenever this update method is called, the relevant bones are set to the **pos** position. To determine which bone you need to move, it is a good idea to print out the **mesh.skeleton** property to the console. This will list all the bones and their names.
