# Morph Targets Manually
In this example, we’ll create a very simple animation using two **THREE.AnimationClip** objects. The first clip will morph the cube from a size of (2, 2, 2) to a size of (2, 20, 2), and the other clip will animate the cube to a size of (40, 2, 2). You can find the example in `09-morph-targets-manually.html`:

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/09-morph-targets-manually.html"><h3>Code</h3></a>

<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/09-morph-targets-manually.html"><h3>Preview</h3></a>

## Concepts
In this example, you’ve got a menu on the right to control the first **THREE.AnimationClip** and another one that you can use to control the second **THREE.AnimationClip**. When you don’t change anything, you can see from the size of the cube that both of the morph targets are applied simultaneously. The width grows to 40 and the height grows to 20. Before we look at how the options we saw in the previous section affect the animation, we’ll first look at how we created this animation from scratch:

```js
function setupModel() {
    // initial cube
    var cubeGeometry = new THREE.BoxGeometry(2, 2, 2);
    var cubeMaterial = new THREE.MeshLambertMaterial({morphTargets: true, color: 0xff0000});

    // define morphtargets, we'll use the vertices from these geometries
    var cubeTarget1 = new THREE.BoxGeometry(2, 20, 2);
    var cubeTarget2 = new THREE.BoxGeometry(40, 2, 2);

    // define morphtargets and compute the morphnormal
    cubeGeometry.morphTargets[0] = {name: 't1', vertices: cubeGeometry.vertices};
    cubeGeometry.morphTargets[1] = {name: 't2', vertices: cubeTarget2.vertices};
    cubeGeometry.morphTargets[2] = {name: 't3', vertices: cubeTarget1.vertices};
    cubeGeometry.computeMorphNormals();

    var mesh = new THREE.Mesh(cubeGeometry, cubeMaterial);

    // position the cube
    mesh.position.x = 0;
    mesh.position.y = 3;
    mesh.position.z = 0;

    // add the cube to the scene
    scene.add(mesh);
    mixer = new THREE.AnimationMixer( mesh );

    animationClip = THREE.AnimationClip.CreateFromMorphTargetSequence('first', [cubeGeometry.morphTargets[0], cubeGeometry.morphTargets[1]], 1);
    animationClip2 = THREE.AnimationClip.CreateFromMorphTargetSequence('second', [cubeGeometry.morphTargets[0], cubeGeometry.morphTargets[2]], 1);
    clipAction = mixer.clipAction( animationClip ).play();  
    clipAction2 = mixer.clipAction( animationClip2 ).play();

    clipAction.setLoop(THREE.LoopRepeat);
    clipAction2.setLoop(THREE.LoopRepeat);
    // enable the controls
    enableControls()
}
```

Now that we’ve got two **THREE.AnimationClip** objects running at the same time, we can play around with the settings to see what the effect is. The best approach is for you to play around with it, but we’ll have a closer look at the two properties that are most important: weight and timeScale.

## Weight Property
The weight property determines how much the model is affected by this clip. If you set one of the two to 0.5, you’ll immediately see that the effect of that clip is reduced, and if you drop it to 0, you won’t see any effect at all. This is very useful if you want to slowly transition from one animation to another (for which Three.js provides helper functions such as fadeIn, fadeOut, crossFadeFrom, and crossFradeTo).

## TimeScale Property
The timeScale property controls how fast a clip is running. If we set this to 2, that clip will play twice as fast, if we set it to 0, it will pause in the state it is in.
