# Human Parts and Motion

Use for human or humanoid actions after the [shared motion invariants](artwork-rigging.md#shared-motion-invariants). These are production decisions, not a mandatory anatomical rig. Adapt to the reference, view, action, and display size.

## Parts and attachment rules

Account for the regions below; split only those needing independent control. Keep left/right identity separate from near/far draw order. A side view may hide parts that a turn will reveal.

| Region | Control and continuity to preserve |
| --- | --- |
| Root, pelvis, waist, chest | Separate travel from body pose. Join pelvis and chest through a covered or deformable waist; provide overlap for bending and twisting. |
| Neck, head, facial features | Keep the neck attached at both ends; facial features follow head direction. Use separate gaze/expression controls only as needed. |
| Shoulder attachment, upper arm, forearm, hand | Anchor the arm at the torso; put elbow/wrist pivots at the depicted joints. Preserve underarm coverage and directional hand shape. |
| Hip attachment, thigh, lower leg, foot | Keep the hip covered, knee bend readable, and foot direction consistent with the ankle. Add toe/heel control when contact requires it. |
| Hair, clothing, accessories | Assign each attachment to its supporting region. Separate anchored cloth from free ends; avoid giving an entire garment the rotation of one limb. |

Use the existing [clothing and endpoint checks](artwork-rigging.md#clothes-wrists-and-directional-end-parts) for sleeve seams, socks, wrists, and ankles; use the [face checks](artwork-rigging.md#uncovered-face-and-continuous-silhouette) before polishing expressions.

## Select by requested action

| Request | Rig and pose decision | Failure to inspect |
| --- | --- | --- |
| Raise/reach with an arm | Rotate through the arm chain around its shoulder attachment. Keep the torso-side sleeve seam attached; author any shoulder-girdle response separately. | Shoulder/torso dragged by arm keys, open underarm, detached sleeve, collapsed elbow, over-rotated wrist; check lowering too. |
| Bend at the waist / bow | Choose hip hinge, spinal curve, or a combination from the reference. Let chest, neck, and head follow the intended chain; keep waist coverage through compression and extension. | Background wedge between chest and pelvis, stretched belt, pinched abdomen, back contour break, feet sliding as the pelvis moves. |
| Twist / look around | Distribute the turn among pelvis, chest, neck, and head as appropriate. Use deformation or alternate artwork where perspective changes cannot be expressed by flat rotation. | Face features staying front-facing, neck tears, wrong near/far arm order, rigid torso rotated like a sign. |
| Squat / sit / stand / jump | Coordinate hips, knees, ankles, and body balance. Keep contacts until intentional lift-off; establish chair/ground contact and landing compression. | Pelvis separating from thighs, unreachable foot targets, seat penetration, floating support, landing without weight. |
| Grasp / carry / push | Establish hand pose and grip/contact before moving the object. Keep hand and contact point aligned through the chain; coordinate both arms for two-handed holds. | Prop drifting in the hand, wrist compensating for a bad arm pose, a snap when attaching or releasing. |

“Keep the shoulder still” should hold the requested attachment/control, not accidentally freeze every region involved in an overhead reach. Natural arm elevation includes scapular motion ([AAOS anatomy](https://www.orthoinfo.org/diseases--conditions/scapular-shoulder-blade-disorders)); represent only what the reference needs and honor an explicit fixed-shoulder brief. The invariant is an intact attachment and deliberate motion, not a universal zero shoulder rotation.

For human idle, walk, run, swim, and drinking, continue with the matching section in [Motion recipes](motion-recipes.md). Those gait recipes describe biped motion; use [Animal motion](animal-motion.md) for quadrupeds. In every action, inspect the attachment and silhouette both with and without occluding arms/props, restoring visibility afterward.

Before handoff, apply the [final spatial and structural consistency pass](verification.md#final-spatial-and-structural-consistency-pass) to the assembled character. For example, inspect the far hand's full path against the torso, garment hem, and legs; a correct front arm does not establish correct visibility of the other arm.
