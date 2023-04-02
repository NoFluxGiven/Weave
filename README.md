# Weave
No frills variable interpolation for Gamemaker.

# Concept
Set up Animators to automatically update their target instance's variables over time.

```
// Create Event
animator = new WeaveAnimator({ rate: 0.5 });

animator.interpolator = function( anim ) {
    return lerp( anim.currentValue, anim.targetValue, anim.options.rate );
}
```

```
// Step Event
animator.update()

if (mouse_check_pressed( mb_left )) {
    animator.animate.x = mouse_x;
    animator.animate.y = mouse_y;
}
```

This will lerp the owning instance's `x` and `y` variables until they reach the target value.

If you want to set the value directly, you can use `stop()` to immediately stop all values from animating:

```
animator.stop();

x = 250;
y = 250;


```

To immediately resolve all animations, just use `skip()`:

```
animator.animate.x = 200;

// Skip the lerping for some reason
animator.skip();
```

# Functions, Methods and Fields

`new WeaveAnimator( options: Struct )` (constructor)

A constructor for a WeaveAnimator instance. The options you provide are stored in the `options` field on the instance.

---

`WeaveAnimator.interpolator` (field)

A field containing a function (see below) - your `WeaveAnimator` will run this function for any tracked variables during `update()`. Use `animator.currentValue`, `animator.targetValue`, and `animator.startValue` to access animation context quickly.

**Example**:

```
animator = new WeaveAnimator({ rate: 0.5 });

animator.interpolator = function( anim ) {
    return lerp( anim.currentValue, anim.targetValue, anim.options.rate );
}
```

---

`WeaveAnimator.currentValue` (field)

A helper field that exposes the current value of the variable being updated at this point in the `update()` function's execution. Essentially sugar for `WeaveAnimator.context[$ "variableHere"].currentValue` - should be used in your interpolator function.

---

`WeaveAnimator.targetValue` (field)

A helper field that exposes the target value of the variable being updated at this point in the `update()` function's execution. Essentially sugar for `WeaveAnimator.context[$ "variableHere"].targetValue` - should be used in your interpolator function.

---

`WeaveAnimator.startValue` (field)

A helper field that exposes the start value of the variable being updated at this point in the `update()` function's execution. Essentially sugar for `WeaveAnimator.context[$ "variableHere"].startValue` - should be used in your interpolator function.

---

`WeaveAnimator.context` (field)

A field containing the context values (start, current and target value) for every tracked variable, for more fine-grained control.

**Example:**

```
x = 0;

animator.animate.x = 70

show_debug_message( animator.context.x.startValue );   // 0
show_debug_message( animator.context.x.targetValue );  // 70
show_debug_message( animator.context.x.currentValue ); // 0
```

---

`WeaveAnimator.animate` (field)

A struct containing fields to interpolate. Your `WeaveAnimator` instance will track changes to these variables, and update them according to your `interpolator` function.

**Example:**

```
animator = new WeaveAnimator({ rate: 0.5 });

// Step towards by 1 per frame
animator.interpolator = function( anim ) {
    return anim.currentValue + sign( anim.targetValue - anim.currentValue )
}

// This will take 50 frames to complete
animator.animate.life -= 50;
```

---

`WeaveAnimator.update()` (method)

Run this in the step event.

---

`WeaveAnimator.stop()` (method)

Immediately ends all currently running interpolations.

---

`WeaveAnimator.skip()` (method)

Immediately skips all currently running interpolations, setting any variables to their target value.

# Configuration

## Proxy Variables - `USE_WEAVE_PROXY_VARIABLES`

Animations act **directly** on their target variables. If you need to know the "true" value (that is, the end result of the animation), you will need to access `context`, which can be cumbersome (`animator.context.x.targetValue`). If `USE_WEAVE_PROXY_VARIABLES` is set to true, you can access them with `weave<type>__<variable>`, for instance `weavetarget__x`.

## Animate Proxies - `ANIMATE_PROXY_VARIABLES`

This modifies the behaviour of your `WeaveAnimators`, causing them to act on `weave__x` instead of `x`, for instance. This is an advanced option, as you'll need to manually update the actual values. All of the other functionality remains the same.

## 
