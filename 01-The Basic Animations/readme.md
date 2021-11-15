# The Basic Animations
In this part, we are going to learn how to animate a simple scene. We will render a cube,
a cylinder, and a ball in a simple animation loop. 
The working code can be found in the file named `01-basic-animation.html`.

<a href="https://github.com/cg2021c/
         threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master
         /src/chapter-09/01-basic-animation.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/
         threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master
         /src/chapter-09/01-basic-animation.html"><h3>Preview</h3></a>

## Concepts
In making an animation, first let's discuss about the **render loop**. In Three.js we have
to render or redraw the scene in a certain interval.
For rendering, we use the standard HTML5 *requestAnimationFrame* functionality like the code below
```js

render();
function render() {
// render the scene
renderer.render(scene, camera);
// schedule the next rendering using requestAnimationFrame
requestAnimationFrame(render);
}
```
With this function, especially with the *requestAnimationFrame* we don't tell the browser
when it needs to update update the screen, unlike with *setInterval(function, interval)*
or with *setTimeout(function, interval)*. By doing that, we can **avoid high CPU usage
due to the browser rendering the scenes in in-opportune time**.

## Animating It

with that approach, we can easily animate objects and change their rotation, scale, position,
material, etc. We don't need to worry because Three.js will render the changes for us
in the next render frame.

For our example, we have these options in our render loop so that we can control the
properties of objects in our animation scene.
```js
function render() {
  cube.rotation.x += controls.rotationSpeed;
  cube.rotation.y += controls.rotationSpeed;
  cube.rotation.z += controls.rotationSpeed;
  step += controls.bouncingSpeed;
  sphere.position.x = 20 + ( 10 * (Math.cos(step)));
  sphere.position.y = 2 + ( 10 * Math.abs(Math.sin(step)));
  scalingStep += controls.scalingSpeed;
  var scaleX = Math.abs(Math.sin(scalingStep / 4));
  var scaleY = Math.abs(Math.cos(scalingStep / 5));
  var scaleZ = Math.abs(Math.sin(scalingStep / 7));
  cylinder.scale.set(scaleX, scaleY, scaleZ);
  renderer.render(scene, camera);
  requestAnimationFrame(render);
}
```

With those functionalities in our render loop, it shows the concept behind the basic
animations. But that's not enough, in order to have a more interactive scene,
sometimes we need to be able to select objects on our screen with our mouse.
