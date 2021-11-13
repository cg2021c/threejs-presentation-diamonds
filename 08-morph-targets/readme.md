# Morph Targets
Morph targets are the most straightforward way of defining an animation. You define all the vertices for each important position (also called keyframes) and tell Three.js to move the vertices from one position to the other. The disadvantage of this approach, though, is that for large meshes and large animations, the model files will become very large. The reason is that for each key position, all the vertex positions are repeated.

Before we dive into the examples, first we’ll look at the three core classes that you use to animate with Three.js. Further on in this chapter, we’ll show you all the functions and properties provided by these objects:

|Core Class|Description|
|----------|-----------|
|**THREE.AnimationClip**|When you load a model that contains animations, you can look in the response object for a field usually called animations. This field will contain a list of **THREE.AnimationClip** objects. A **THREE.AnimationClip** most often holds the data for a certain animation or activity the model you loaded can perform. For instance, if you loaded a model of a bird, one **THREE.AnimationClip** would contain the information needed to flap the wings, and another one might be opening and closing its beak.|
|**THREE.AnimationMixer**|**THREE.AnimationMixer** is used to control a number of **THREE.AnimationClip** objects. It makes sure the timing of the animation is correct, and makes it possible to sync animations together, or cleanly move from one animation to another.|
|**THREE.AnimationAction**|**THREE.AnimationMixer** itself doesn’t expose a large number of functions to control the animation, though. This is done through **THREE.AnimationAction** objects, which are returned when you add a **THREE.AnimationClip** to a **THREE.AnimationMixer** (or you can get them at a later time, by using functions provided by **THREE.AnimationMixer**).|

For this first morphing example, we use a model that is also available from the Three.js distribution: the horse. The easiest way to understand how a morph targets-based animation works is by opening up the `08-morph-targets.html` example. The following links show a code and a preview of this example:

<a href="https://github.com/cg2021c/threejs-presentation-diamonds/blob/main/Learn-Three.js-Third-Edition-master/src/chapter-09/08-morph-targets.html"><h3>Code</h3></a>

<a href="https://cg2021c.github.io/threejs-presentation-diamonds/Learn-Three.js-Third-Edition-master/src/chapter-09/08-morph-targets.html"><h3>Preview</h3></a>

## Concepts
In older versions of Three.js, we had to use specific meshes (for example **THREE.MorphAnimMesh** or **THREE.MorphBlendMesh**) to work with animations. In the later versions of Three.js, you can use the normal **THREE.Mesh** objects to create your meshes and place them in a scene. To run an animation, we now use the **THREE.AnimationMixer**:

```js
var loader = new THREE.JSONLoader();
loader.load('../../assets/models/horse/horse.js', function (geometry, mat) {
    geometry.computeVertexNormals();
    geometry.computeMorphNormals();

    var mat = new THREE.MeshLambertMaterial({morphTargets: true, vertexColors: THREE.FaceColors});
    mesh = new THREE.Mesh(geometry, mat);
    mesh.scale.set(0.15,0.15,0.15);
    mesh.translateY(-10);
    mesh.translateX(10);

    mixer = new THREE.AnimationMixer( mesh );
    // or create a custom clip from the set of morphtargets
    // var clip = THREE.AnimationClip.CreateFromMorphTargetSequence( 'gallop', geometry.morphTargets, 30 );
    animationClip = geometry.animations[0] 
    clipAction = mixer.clipAction( animationClip ).play();    

    clipAction.setLoop(THREE.LoopRepeat);
    scene.add(mesh)

    // enable the controls
    enableControls()
})
```

With a **THREE.AnimationMixer**, we can control the animations of a single object (although, if you want, you can have a single **THREE.AnimationMixer** control multiple objects).

At this point, though, when we render the scene, the animation won’t play yet. For this, we have to make a small change to our render loop:

```js
function render() {
    stats.update();
    var delta = clock.getDelta();
    trackballControls.update(delta);
    requestAnimationFrame(render);
    renderer.render(scene, camera)

    if (mixer && clipAction) {
        mixer.update( delta );
        controls.time = mixer.time;
        controls.effectiveTimeScale = clipAction.getEffectiveTimeScale();
        controls.effectiveWeight = clipAction.getEffectiveWeight();
    }
}
```

## THREE.AnimationClip
|Name|Description|
|----|-----------|
|duration|The duration of this track (in seconds).|
|name|The name of this clip. In the case of our horse, this might be "gallop".|
|tracks|The internal property used to keep track of how certain properties of the model are animated.|
|uuid|This unique ID of this clip. This is assigned automatically.|
|optimize()|This optimizes the **THREE.AnimationClip**.|
|resetDuration()|This determines the correct duration of this clip.|
|trim()|This trims all the internal tracks to the duration set on this clip.|
|CreateClipsFromMorphTargetSequences(name, morphTargetSequences, fps, noLoop)|This used internally by the **THREE.JsonLoader** to create a list of **THREE.AnimationClip** instances based on a set of morph-target sequences.|
|CreateFromMorpTargetSequences(name, morphTargetSequence, fps, noLoop)|This creates a single **THREE.AnimationClip** from a sequence of morph-targets.|
|findByName(objectOrClipArray, name)|Searches for **THREE.AnimationClip** by name.|
|parse and toJson|Allows you to restore and save a **Three.AnimationClip** as JSON.|
|parseAnimation|Used internally to create a **THREE.AnimationClip**.|

## THREE.AnimationMixer
|Name|Description|
|----|-----------|
|AnimationMixer(rootObject)|The constructor for this object. This constructor takes a **THREE.Object3D** as an argument (for example, a **THREE.Mesh** of a **THREE.Group**).|
|.time|The global time for this mixer; this starts at 0, at the time when this mixer is created.|
|.timeScale|The timescale can be used to speed up or slow down all the animations managed by this mixer. If the value of this property is set to 0, all the animations are effectively paused.|
|.clipAction(animationClip, optionalRoot)|This creates a **THREE.AnimationAction** that can be used to control the passed-in **THREE.AnimationClip**. If the animation clip is for a different root object, you can pass that in as well.|
|.existingAction(animationClip, optionalRoot)|This returns the **THREE.AnimationAction** that can be used to control the passed-in **THREE.AnimationClip**. Once again, if the **THREE.AnimationClip** is for a different rootObject, you can also pass that in.|

## THREE.AnimationAction
|Name|Description|
|----|-----------|
|clampWhenFinished|When set to true, this will cause the animation to be paused when it reaches its last frame. The default is false.|
|enabled|When set to false, this will disable the current action so that it has no effect on the model. When the action is re-enabled, the animation will continue where it left off.|
|loop|The looping mode of this action (which can be set using the setLoop function). This can be set to the following:<ul><li>**THREE.LoopOnce**: Plays the clip only one time.</li><li>**THREE.LoopRepeat**: Repeats the clip based on the number of repetitions that have been set.</li><li>**THREE.LoopPingPong**: Plays the clip based on the number of repetitions, but alternates between playing the clip forward and backward.</li></ul>|
|paused|Setting this property to true will pause the execution of this clip.|
|repetitions|Number of times to repeat the animation. Used by the loop property. The default is Infinity.|
|time|The time this action has been running; this is wrapped from 0 to the duration of the clip.|
|timeScale|The timescale can be used to speed up or slow down this animation. If the value of this property is set to 0, this animation is effectively paused.|
|weight|The effect this animation has on the model from a scale of 0 to 1. When set to 0, you won’t see any transformation of the model from this animation, and when set to 1, you see the full effect of this animation.|
|zeroSlopeAtEnd|When set to true (which is the default), this will make sure there is a smooth transition between separate clips.|
|zeroSlopeAtStart|When set to true (which is the default), this will make sure there is a smooth transition between separate clips.|
|crossFadeFrom(fadeOutAction, durationInSeconds, warpBoolean)|Causes this action to fade in, while the fadeOutAction is faded out. The total fade takes durationInSeconds time. This allows for smooth transitions between animations. With the warpBoolean set to true, it will apply additional smoothing of timescales as well.|
|crossFadeTo(fadeInAction, durationInSeconds, warpBoolean)|Same as crossFadeFrom, but this time fades in the provided action, and fades out this action.|
|fadeIn(durationInSeconds)|Increases the weight property slowly from 0 to 1, within the passed time interval.|
|fadeOut(durationInSeconds)|Decreases the weight property slowly from 0 to 1 within the passed time interval.|
|getEffectiveTimeScale()|Returns the effective timescale based on the currently running warp.|
|getEffectiveWeight()|Returns the effective weight based on the current running fade.|
|getClip()|Returns the **THREE.AnimationClip** this action is managing.|
|getMixer()|Returns the mixer that is playing this action.|
|getRoot()|Gets the root object which is controlled by this action.|
|halt(durationInSeconds)|Gradually decreases the timeScale to 0 within the durationInSeconds.|
|isRunning()|Checks whether the animation is currently running.|
|isScheduled()|Checks whether this action is currently active in the mixer.|
|play()|Starts running this action (starting the animation).|
|reset()|Resets this action. This will result in setting paused to false, enabled to true, and time to 0.|
|setDuration(durationInSeconds)|Sets the duration of a single loop. This will change the timeScale so that the complete animation can play within the durationInSeconds.|
|setEffectiveTimeScale(timeScale)|Sets the timeScale to the provided value.|
|setEffectiveWeight()|Sets the weight to the provided value.|
|setLoop(loopMode, repetitions)|Sets the loopMode and the number of repetitions. See the loop property for the options and their effect.|
|startAt(startTimeInSeconds)|Delays starting the animation for startTimeInSeconds.|
|stop()|Stops this action, and reset is applied.|
|stopFading()|Stops any scheduled fading.|
|stopWarping()|Stops any schedule warping.|
|syncWith(otherAction)|Syncs this action with the passed-in action. This will set this actions time and timeScale value to the passed-in action.|
|warp(startTimeScale, endTimeScale, durationInSeconds)|Changes the timeScale property from the startTimeScale to the endTimeScale within the specified durationInSeconds.|
