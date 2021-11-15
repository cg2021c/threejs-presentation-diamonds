# Bones Manually

Morph animations are very straightforward. Three.js knows all the target vertex positions and only needs to transition each vertex from one position to the next. For bones and skinning, it becomes a bit more complex. When you use bones for animation, you move the bone, and Three.js has to determine how to translate the attached skin (a set of vertices) accordingly. 

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
