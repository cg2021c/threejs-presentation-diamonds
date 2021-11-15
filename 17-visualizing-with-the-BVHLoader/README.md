# Visualizing a skeleton with the BVHLoader
The BVHLoader is a slightly different loader than the once we have we’ve
seen so far. This loader doesn’t return meshes or geometries with animations
but just returns a skeleton and an animation. An example of this is shown in
`17-animation-from-bvh.html`

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/17-animation-from-bvh.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/17-animation-from-bvh.html"><h3>Preview</h3></a>

## Concepts
To visualize this, we can reuse the `THREE.SkeletonHelper`
```js

var loader = new THREE.BVHLoader();
  loader.load('../../assets/models/amelia-dance/DanceNightClub7_t1.bvh', function (result, mat) {
      skeletonHelper = new THREE.SkeletonHelper( result.skeleton.bones[ 0 ] );
      // allow animation mixer to bind to SkeletonHelper directly
      skeletonHelper.skeleton = result.skeleton;
      var boneContainer = new THREE.Object3D();
      boneContainer.translateY(-70);
      boneContainer.translateX(-100);
      boneContainer.add( result.skeleton.bones[ 0 ] );
      scene.add( skeletonHelper );
      scene.add( boneContainer );
      mixer = new THREE.AnimationMixer( skeletonHelper );
      clipAction = mixer.clipAction( result.clip ).setEffectiveWeight( 1.0 ).play();
  })
```
