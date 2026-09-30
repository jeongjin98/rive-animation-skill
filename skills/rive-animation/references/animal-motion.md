# Animal Parts and Motion

Use after the [shared motion invariants](artwork-rigging.md#shared-motion-invariants). Select body structure and action before building a rig. These are production judgments; species, proportions, stylization, and the motion reference govern the result. Do not reuse a human walk by adding two more legs.

## Body structures

| Structure | Parts and attachments to account for |
| --- | --- |
| Quadruped | Travel root, pelvis, flexible trunk/chest, neck/head, four distinct limb chains and feet, optional tail/ears. Separate fore/hind and left/right identity; keep near/far occlusion independent. |
| Winged animal | Torso, neck/head, wing roots and articulated wing sections, tail, feet. Preserve attachment and wing surface while folding; bird and bat structures need different artwork. |
| Fish / aquatic body | Head/body, deformable trunk/tail, independently needed fins. Preserve a continuous silhouette and distinguish propulsion from steering. |
| Serpentine / many-legged body | Continuous deformable trunk or connected segments; add leg pairs only when visible motion needs them. Preserve length/attachment through curves. |

Use the visible anatomy to locate joints; do not treat an animal's hock as a backward human knee. Keep chest-to-forelimb, pelvis-to-hindlimb, neck, and tail-root surfaces closed at maximum reach and compression. Split images only as needed; flexible trunks can remain continuous weighted artwork.

## Quadruped walk and run

First label the feet LF/RF/LH/RH (left/right fore/hind). Record stance and swing intervals in a small contact chart before keying body bob. Choose the gait from the species/reference; if “run” is underspecified, state a plausible choice instead of treating all fast motion as one cycle.

| Gait | Contact pattern to establish | Motion checks |
| --- | --- | --- |
| Walk | Four separately timed footfalls with continuous ground support. A common lateral-sequence example is LH → LF → RH → RF; use the depicted species' sequence. | Weight transfers over supporting feet, swing feet clear the floor, and each planted foot follows the contacted surface. Avoid airborne pauses or four legs swinging together. |
| Trot | Alternating diagonal pairs: LF+RH, then RF+LH. | Match pair contacts and body compression; any suspension follows the reference speed. Do not accidentally use same-side pairs. |
| Pace / amble | Pace alternates same-side pairs; an amble can separate those contacts. | Use only when appropriate to the species/reference; retain its timing instead of “correcting” it into a trot. |
| Canter | Asymmetric three-beat pattern in the usual horse example: RH → LH+RF → LF → suspension for left lead; mirror for right lead. | Record the lead and check the diagonal pair, support changes, and return to the next stride. Do not mirror half a trot cycle. |
| Gallop | Asymmetric running with separate footfalls and reference-specific suspension. | Establish lead, gathered/extended poses, propulsion, and landing. Match spinal flexion and transverse/rotary pattern to the species; do not impose a horse cycle on a dog or cat. |

Equine gait distinctions are grounded in [Cooperative Extension's gait overview](https://horses.extension.org/horse-movement-and-way-of-going/), [natural gait descriptions](https://horses.extension.org/horse-natural-gaits/), and [canter footfall sequence](https://horses.extension.org/horse-canter/). They are examples, not a universal footfall prescription for all quadrupeds.

Build foot contacts and root travel first; then coordinate chest/pelvis pitch, spine, and head response. Add ears, mane, and tail afterward. More speed may require a new gait, stride, stance duration, and body action; merely speeding up walk keys is insufficient.

For travel, match root speed to stance-foot motion in scene space. For a scrolling floor, match contact to the floor's flow. Keep an in-place demonstration explicitly in place. Check far legs behind near legs, clearance under the trunk, paw/hoof direction, every touchdown/lift-off, and the stride boundary in playback.

## Other actions and transitions

| Action | Decision and failure checks |
| --- | --- |
| Start / stop / turn / change gait | Shift support and balance before acceleration or braking. Reconcile contact timing during transitions; blending two cycles must not drag planted feet. Turning should preserve limb identity and intentional lead changes. |
| Jump / hop / land | Establish preparation, push-off, flight, and landing with the species' contact order. Check tucked-limb clearance, belly continuity, reach, and landing compression. |
| Sit / lie down / stand | Stage support transfers and limb folding. Keep chest/belly/pelvis continuous; inspect floor penetration, trapped legs, and weight transfer back to the feet. |
| Idle / breathe / look / wag | Keep supporting paws stable. Separate breathing, head/gaze, ears, and tail response; a wag starts at an attached tail root and must not rotate the whole pelvis accidentally. |
| Flap / glide / fold wings | Coordinate root rotation, elbow/wrist folding, and wing surface deformation. Inspect upstroke/downstroke, body response, and feather/membrane overlap; a rigid paddle rotation is insufficient when folding is visible. |
| Swim | Choose trunk/tail waves or limb strokes for the animal. Keep fins and tail attached; connect propulsion to travel and scoped water response, not just whole-body bobbing. |
| Slither / crawl | Propagate bends or leg phases along the body while preserving contact and intended length. Check corners for pinching, segment gaps, and feet sliding against the floor. |

Inspect the requested action and any requested entry/exit, not every gait in this guide. Use [Verification](verification.md) for structural versus actual playback evidence; these recipes are not validated animation assets.
