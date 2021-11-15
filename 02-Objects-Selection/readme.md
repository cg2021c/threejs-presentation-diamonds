# Objects Selection
Since we are going to discuss about animations and cameras, why not talk about objects selection.
First, let's check out the preview below before looking at the codes and concepts.

Here is the codes and preview. The files can be found in ```02-selecting-objects.html```

<a href="https://github.com/cg2021c/
         threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master
         /src/chapter-09/02-selecting-objects.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/
         threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master
         /src/chapter-09/02-selecting-objects.html"><h3>Preview</h3></a>


## Concepts
Here we will learn how can our clicks translate to in-scene actions. For that, we can use
the code below.

```js
var projector = new THREE.Projector();
function onDocumentMouseDown(event) {

  var vector = new THREE.Vector3((event.clientX / window.innerWidth) * 2 - 1, -(event.clientY / window.innerHeight) * 2 + 1, 0.5);
  vector = vector.unproject(camera);
  
  var raycaster = new THREE.Raycaster(camera.position, vector.sub(camera.position).normalize());
  var intersects = raycaster.intersectObjects([sphere, cylinder, cube]);
  
  if (intersects.length > 0) {
    console.log(intersects[0]);
    intersects[0].object.material.transparent = true;
    intersects[0].object.material.opacity = 0.1;
  }
}
```
There, we use the `THREE.Projector` in tandem with `THREE.Raycaster` to know whether we clicked
on a specific object. The events that happened when we clicked on the screen is as follows :

1. `THREE.Vector3` is created based on the position where clicked on the screen.
2. Next, `vector.unproject` function will convert the clicked position 
on screen to coordinates in our Three.js scene. Or we can say, we "placed" the clicked position
from a screen projection, to the 3D-World of the scene.
3. Next, we create `THREE.Raycaster`. `THREE.Raycaster` will cast rays to
our scene. Here, we emit a ray from the position of the camera
(`camera.position`) to our clicked position.
4. We use the `raycaster.intersectObjects` function to know if we hit any objects using our ray.

