# Weave
No frills variable interpolation for Gamemaker.

# Concept
Set up Animators to automatically update their target instance's variables over time.

```
// Create Event
animator = new WeaveAnimator({ rate: 0.5 });

animator.interpolator = function( anim ) {
    return lerp( anim.currentValue, anim.targetValue, anim.params.rate );
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





