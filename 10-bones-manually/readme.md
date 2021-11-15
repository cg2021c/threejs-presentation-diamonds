# Bones Manually

Morph animations are very straightforward. Three.js knows all the target vertex positions and only needs to transition each vertex from one position to the next. For bones and skinning, it becomes a bit more complex. When you use bones for animation, you move the bone, and Three.js has to determine how to translate the attached skin (a set of vertices) accordingly. For this example, we use a model that was exported from Blender to the Three.js format (hand-1.js in the models/hand folder). This is a model of a hand, complete with a set of bones. By moving the bones around, we can animate the complete model. Let’s first look at how we loaded the model:

```loader.load('../../assets/models/hand/hand-1.js', function (geometry, mat) {
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
