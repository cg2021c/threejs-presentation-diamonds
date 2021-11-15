# Animation with Tween.js
We've learned how to animate stuff and click the objects. But sometimes animating can be
a hard, time consuming task. For that, we can use Tween.js to help us. Tween.js is a small JavaScript
library that you can use to easily define transition of a property between two values.
You can download the Tween.js library from <a href="https://github.com/sole/tween.js/">this link</a>
All the intermediate points between the start and end values are calculated for you.
The process of doing so is called **tweening** or **inbetweening**.

Still don't get it?

Here is a famous (or infamous) tweening.

![Tweening-Scene](https://c.tenor.com/xYeRwzgV8SoAAAAd/naruto-shipudden.gif)


That is the scene, and a tween or in-between, is an image that sits between keyframes of animation.

![Tweening-Example](https://user-images.githubusercontent.com/65166398/141770189-83472d31-6561-4f34-82b8-8b2942562820.png)


Let's see the example below. The codes can be found in the file ```03-animation-tween.html```

<a href="https://github.com/cg2021c/
         threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master
         /src/chapter-09/03-animation-tween.html"><h3>Code</h3></a>
 
 
<a href="https://cg2021c.github.io/
         threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master
         /src/chapter-09/03-animation-tween.html"><h3>Preview</h3></a>

## Concepts
For example, we want to change the position of a mesh from a point to another point in 10 seconds
we can use the code as follows :
```js
var tween = new TWEEN.Tween({x: 10}).to({x: 3}, 10000)
  .easing(TWEEN.Easing.Elastic.InOut)
  .onUpdate( function () {
    // update the mesh
  })
```
In the example above, we create ```TWEEN.Tween```. This tween will make sure that the
*x* property is changed from 10 to 3 in 10000 milliseconds. Aside from that, Tween.js can
also help you to define how this property is changed. The way the value is changed over time is called
**easing**. You can do linear, quadratic, or other types of easing. For more information about easing possibilities,
you can go to <a href="http://sole.github.io/tween.js/examples/03_graphs.html">this link</a>

In our preview, we have used the tweening to move our points into a single point. We can transition the particles using
a **tween** created with the Tween.js library as follows :

```js

var posSrc = { pos: 1}
var tween = new TWEEN.Tween(posSrc).to({pos: 0}, 2000);
tween.easing(TWEEN.Easing.Bounce.InOut);
var tweenBack = new TWEEN.Tween(posSrc).to({pos: 1}, 2000);
tweenBack.easing(TWEEN.Easing.Bounce.InOut);
tweenBack.chain(tween);
tween.chain(tweenBack);
tween.start();
var onUpdate = function () {
  var count = 0;
  var pos = this.pos;
  loadedGeometry.vertices.forEach(function (e) {
    var newY = ((e.y + 3.22544) * pos) - 3.22544;
    particleCloud.geometry.vertices[count++].set(e.x, newY, e.z);
  });
  particleCloud.sortParticles = true;
};
```

With the code above, we have created 2 tweens, namely ```tween``` and ```tweenback```. The first
one is to define the transition from 1 to 0 (from original to the single point), and the second one
is to do the opposite. To loop the two tween, we can use the ```chain()``` function.
To access the updated value of the tween, we can either use ```onUpdate``` function from this library,
or we can access the updated values directly (this is what we do in this example).

Before we render the tween, what if we want to store the original values? we can do so by
storing the original positions of the vertices using this code below :
```js
// copy the original position, so we can referene that when tweening
var origPosition = geometry.attributes['position'].clone()
geometry.origPosition = origPosition
```
By doing that, if we want to access the original positions, we can look at it from
```origPosition``` variable on the geometry.

Now that we have a reference to the original positions, we can use that value to calculate
new positions based on the values of the ```tween```. In our ```render``` function, we can add the following :

```js
TWEEN.update();
var positionArray = mesh.geometry.attributes['position']
var origPosition = mesh.geometry.origPosition

for (i = 0; i < positionArray.count ; i++) {
  var oldPosX = origPosition.getX(i);
  var oldPosY = origPosition.getY(i);
  var oldPosZ = origPosition.getZ(i);
  positionArray.setX(i, oldPosX * posSrc.pos);
  positionArray.setY(i, oldPosY * posSrc.pos);
  positionArray.setZ(i, oldPosZ * posSrc.pos);
}
positionArray.needsUpdate = true;

```

In the ```render``` function, firstly we call the ```TWEEN.Update```, which calculates the new
values of the tweened variable (from 1 to 0 and vice versa). Remember that we use ```posSrc.pos```
variable for this. Then, we iterate over all the vertices and update their respective positions.

With the steps above, ```tween``` library will take care of the positioning across various points on the screen.
So, with the help of this library, we can more easily handle the tweening process of our animation.

