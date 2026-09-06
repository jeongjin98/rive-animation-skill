# Motion Recipes

Read this guide for timelines and scene motion. The sequences and numbers illustrate production decisions. No `.riv` asset verified through actual creation and playback is included. Adapt the examples to the reference and request.

## Common Workflow

- First inspect the still image's composition, silhouette, and gaze. For a small change, modify only what is needed.
- Refine key poses, timing, intermediate poses/interpolation, and secondary motion in that order. Readable action matters more than a large number of keys.
- Timing is the duration of an action; spacing is the change in position between moments. Connect trajectories, anticipation, the weight of landings/stops, and follow-through to the scene's emotion.
- Keep steady movement at a steady speed. Shape landings, reactions, and deceleration according to the forces involved. Do not apply the same easing to every key.
- Give the body, head, clothing, and props distinct amplitudes and delays. Moving everything with the same sine wave does not replace weight shifts or walking.
- For continuous loops, check boundary velocity and direction as well as values. Place discontinuous swaps where they are actually hidden, such as behind an occluder or outside the frame.

Check the current features in [Keys](https://rive.app/docs/editor/animate-mode/keys), [Timeline](https://rive.app/docs/editor/animate-mode/timeline), and [Interpolation](https://rive.app/docs/editor/animate-mode/interpolation-easing). Hold can switch values; Linear can produce a constant rate of change. For overshoot, check the differences between Cubic, Cubic Value, and Elastic and the available tool support.

## Character Idle

1. Establish balance and expression in the reference pose.
2. Add subtle movement around the breathing center, such as the torso or shoulders. Do not scale the face, hands, and floor together.
3. Give blinks and gaze shifts their own timing, with anticipation and holds that fit the emotion.
4. Let hair, clothing, and props follow the main motion with a delay and settle. Start by testing small amplitudes.
5. View the initial entry and several cycles at the intended display size. Check for facial distortion or all parts bobbing in unison.

If the character stretches like rubber, separate the breathing control instead of scaling the whole body. If the expression changes, compare eye and mouth shapes and the reference gaze pose first.

## Character Walk

Inputs are artwork with separable or deformable parts, a floor, the movement mode, emotion, and the purpose of the repetition. Choose the required rig using [Artwork and Rigging](artwork-rigging.md). Specify camera, character root, and environment movement separately.

| Movement mode | Contact and looping |
| --- | --- |
| The character advances through the scene | Match root advancement and local foot movement so the supporting foot stays fixed in scene coordinates. Design the gait cycle separately from the scene's repeating route or hidden reset. |
| The character stays in place on screen while the environment moves | Match the foot's screen movement during contact to the floor/environment flow. Specify the camera treatment separately. |
| A standalone asset demonstrates an in-place walk | Make the in-place intent explicit. Do not describe it as physically advancing over a stationary scene floor. |

1. Establish Contact → Down → Passing → Up and the opposite foot's half-cycle. Follow the reference's stride, speed, and lean.
2. Match pelvis weight shifts and foot trajectories first. Distinguish toe/heel action and contact/swing phases.
3. With IK, test foot target space, knee direction, and reach limits. With FK, adjust joint rotations together with the foot path.
4. Add opposing arm motion and upper-body response. Adjust hands, props, head, and clothing afterward.
5. Check draw order at crossing poses, joint overlap, and contact shadows.
6. Inspect the first and last poses and boundary velocity, then play the loop repeatedly. Add emotional detail once the basic gait is stable.

For foot sliding, investigate contact and root/environment speed. For knee pops, check reach, bend direction, and interpolation. For elbow gaps, check pivots, hierarchy, overlap, and weight consistency across a continuous joint surface bound to the same bones. For occlusion errors, examine [Draw Order](https://rive.app/docs/editor/animate-mode/animating-draw-order) before dismantling the rig.

## Scene Loops with Backgrounds

1. Define the overall composition and moving elements. Also specify which elements stay still.
2. Compose the foreground, middle ground, background, character, shadows, and occlusion. Distinguish effects on a single image from motion of individual elements.
3. Complete the main action and check that character behavior and background speed/direction describe the same situation.
4. For repeating backgrounds, design adjoining tiles or hidden reset regions. The edge artwork must match as well as tile width and travel distance.
5. Add secondary motion and compare occlusion, color, and shadows immediately before and after the loop boundary.

A fixed camera can still show scenery moving past a train window or leaves swaying in the wind. Do not remove environmental motion merely because the camera is fixed.

## Concrete Design Example: A Four-Second Window Scene

Brief: a fixed camera and window frame, a seated character by the window, passing scenery, one blink and a gaze shift, and a four-second overall loop. The values below are starting points for design.

Example hierarchy: Scene → LandscapeFar / LandscapeNear / WindowFrame / Character / Foreground. Clip the landscape to the window area and keep the head and eyes in independently editable groups or controls.

| Element | At 0 seconds | During the loop | At 4 seconds and repeat |
| --- | --- | --- | --- |
| Camera, window frame, and character base position | Reference composition | Unchanged | Unchanged |
| Distant landscape | First tile position | Move at a constant speed | The next tile position with the same appearance |
| Nearby landscape | First tile position | Move faster than the distant landscape | Join according to its own tile cycle |
| Eyes | Open | For example, briefly close and reopen around 0.8 seconds | Open reference pose |
| Gaze | Reference direction | For example, move/hold between 1.4 and 2.8 seconds | Return to the reference direction before the end |

If the horizontal repeat width is `W`, a segment that travels `W` in one cycle can move at a constant speed of `W / 4 seconds`. Inspect duplicate tiles and their visibility within the clip together. Choose widths and periods for the distant and nearby landscapes that fit the final four-second loop.

Inspect the reference frame, closed eyes, gaze extremes, and the moments immediately before and after four seconds. Play about three consecutive cycles at normal speed to check resets, rhythm, and facial wobble. Three cycles are a recommendation for this example, not a fixed requirement for every task.

Do not assume this example includes a `.rev`, `.riv`, or video. When reusing an example that has actually been created and verified, record its source, usage rights, editable source, playback video, and verification environment together.
