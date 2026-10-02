# Artwork Preparation and Rigging Decisions

Reference date: 2026-09-06. This guide draws on the official Rive documentation; observed tool procedures are maintained in [MCP operations](mcp-operations.md#rigging-tools).
This is a production decision guide. Its examples are not assets verified through creation, export, and playback.
`Official` identifies documented features and `Production judgment` identifies practical recommendations.
If menus or tools have changed, read the current documentation and call schema. Do not invent unverified arguments or operations.

## Shared motion invariants

Read these before the applicable [human](human-motion.md), [object](object-motion.md), or [animal](animal-motion.md) guide. These are production judgments; preserve intentional stylization and separations in the reference.

**Preserve spatial and structural consistency throughout motion.** Before rigging or keying, define which relationships must hold and which intentionally change, with their action phase or transition condition. Apply only relevant relationships; a compact note in the part map is sufficient. Judge assembled subjects and the full scene, not just independently moving parts.

| Relationship | What must remain consistent |
| --- | --- |
| Attachment and continuity | Connected parts remain joined and intended continuous surfaces stay closed through bending and extension. No accidental gaps, detached seams, exposed joint caps, or doubled outlines. |
| Depth and occlusion | Front/back order and visibility agree with the intended spatial arrangement. Overlap in a 2D projection is valid; appearing through a surface that should cover a part is not. Turns and crossings may deliberately change ordering. |
| Shape and volume | Rigid dimensions remain stable; flexible parts preserve their intended volume, silhouette, and markings. Squash/stretch follows the reference rather than accidental inherited scale. |
| Contact and separation | Required contacts follow their supporting surfaces until release; other parts keep appropriate clearance. Avoid unintended penetration or sliding. An intentional grasp, release, or landing changes the contact relationship. |
| Scale and depth | Relative sizes, placement, projection, and apparent scale agree across the scene and through travel. Establish the [scene baseline](#scene-scale-and-perspective) before rigging. |

- Record relevant parents, pivots, rigid/deforming methods, overlaps, and front/back order in the part map. Distinguish what drives, follows, or stays anchored. A control hierarchy need not match draw order; use the correct anchor space and avoid double-applying inherited movement.
- Split only where independent motion or deformation requires it; reuse suitable parts and provide hidden artwork/overlap for the full range. A logical body region need not become a separate image or bone. Repair broken relationships at their cause rather than hiding them with unrelated foreground parts.
- Check these relationships in neutral/extreme poses, intermediate poses in both directions, and immediately before/during/after intentional changes. Repeat the [final consistency pass](verification.md#final-spatial-and-structural-consistency-pass) in actual playback; correct endpoints do not establish a correct transition.

### Scene scale and perspective

- Record a compact scene baseline: projection/camera, horizon and vanishing direction where applicable, ground plane, one familiar size reference, relative sizes, depth placement, and foot/wheel contact anchors. Use the supplied composition; preserve intentional stylization or orthographic/flat views rather than forcing realistic perspective.
- Compare subjects at comparable depth before applying perspective. Check plausible proportions among people, vehicles, doors, furniture, and animals using the depicted types. A person beside a car must not read as a giant beside a toy. Compare appropriate dimensions: a person can be taller than a car's roof; “every car must be taller than every person” is not a valid rule.
- Separate source image dimensions from scene size. Transparent padding, crop bounds, and import scale must not determine how large an object is in the world. Compose all major subjects together in a rough still before polishing their rigs.
- In a perspective scene, apparent size must agree with relative size and depth. A nearby person can appear larger than a distant car, but ground placement, occlusion, and perspective must support that distance. Do not excuse an accidental mismatch by assigning depth that the composition does not show. Orthographic scenes retain scale with depth.
- Place feet, tires, bases, and contact shadows on the intended ground surface. Align road edges, lane markings, building edges, and object orientations with a coherent viewpoint. Judge depth from the ground plane and camera, not screen Y alone; slopes and elevated surfaces need their own placement.
- When motion changes depth, coordinate position, apparent scale, ground contact, occlusion, and projected speed. Sideways travel at constant depth should not grow or shrink without a camera/design reason. Recheck the complete scene at the start, middle, end, and nearest/farthest positions; repair placement or assembly scale before retuning the animation.

## 1. Prepare Artwork for Rigging

- Production judgment: First identify the reference artwork, final display size, moving parts, and hidden areas.
- Prepare surfaces that movement will reveal, such as the torso behind a raised arm or the background behind separated legs.
- When composition, color, silhouette, or texture matters, compare the imported still image with the original before rigging.
- Do not split static backgrounds and props into unnecessary pieces. Separate only what the motion requires.
- Output: a still image, part list, style characteristics to preserve, and required range of motion.

### Uncovered face and continuous silhouette

- Inspect the head without hands, bottles, hair overlays, or other parts hiding the area under review. Restore temporary inspection changes afterward.
- Establish the head, cheek, jaw, ear, and neck outline before placing fine facial features. Check tangent continuity around the nose and lips; accidental notches, protrusions, and stepped joins are artwork defects even when face scale is unchanged.
- Keep eyes, brows, nose, mouth, and ear consistent with the same head direction and perspective. Preserve the reference's intentional asymmetry and stylization.
- Compare neutral and expressive poses enlarged and at final display size. Moving the mouth cannot repair a malformed outer contour. Use a continuous path or well-designed overlaps as appropriate; do not require every face to be one path.

### From stickman to final artwork

- For a motion-first brief, validate timing, contact, and trajectories on the stickman before adding polished artwork.
- Then add simple thick limbs and directional shoes/hands. Test joint volume and inner/outer contours at maximum bend, extension, and transition poses. An accepted line rig does not establish acceptable deformation of a full character.
- Reassess deformation needs at this stage. Use actual bones and bindings when that is the agreed implementation; group names containing “bone” are not bone objects. Use rigid shapes where they work, with weighted or directly deformed connections where continuity requires them.
- Preserve accepted joint positions, timing, and trajectories when replacing proxies. If changed proportions require motion changes, explain and recheck those changes instead of silently rebuilding the action.

## 2. Choose SVG or PSD

Official: [SVG & Vector Assets](https://rive.app/docs/editor/assets/svg), [Photoshop Files](https://rive.app/docs/editor/assets/psd).

| Source | Suitable for | Check immediately after import |
| --- | --- | --- |
| SVG | Artwork whose paths, colors, strokes, and shapes need direct editing | Compare strokes, gradients, and clipping with the original |
| PSD / transparent images | Artwork whose texture and brushwork should be preserved | Check layer positions, transparent edges, and hidden surfaces |

- SVG content becomes Rive shapes, paths, fills, strokes, and groups. The runtime does not interpret the original SVG.
- Embedded images, filters, and skew in SVG are unsupported; masks and dashes may import differently.
- Use Presentation Attributes when exporting from Illustrator. If import problems occur, simplify unsupported features.
- PSD import includes only visible image layers. Renaming a layer before reimport can cause it to be treated as a new layer and break references.
- Production judgment: Keep PSD layer names stable. Do not automatically vectorize artwork whose original style needs to be preserved.

## 3. Hierarchy, Coordinate Spaces, and Pivots

Official: [Transform Spaces](https://rive.app/docs/editor/fundamentals/transform-spaces), [Freeze and Origin](https://rive.app/docs/editor/fundamentals/freeze-and-origin).

- Nested groups and bones create transform spaces. Children inherit their parent's position, rotation, and scale changes.
- Use Freeze to reposition a parent or pivot while preserving its children's visible positions. Check that Freeze is disabled after the adjustment.
- Production judgment: Separate scene movement, character movement, pelvis motion, and joint rotation so they can be controlled independently.
- Test rotations at the shoulder, elbow, and knee pivots. Fix gaps between parts and unexpected transform inheritance first.
- Output: a hierarchy and control list. Record whether targets are controlled in local or world space.

### Clothes, wrists, and directional end parts

- Place shoulder, elbow, wrist, ankle, and toe pivots at their visible anatomical joins. After moving a pivot or reparenting, compensate child transforms to preserve the reference pose and inspect existing animation.
- Separate a sleeve's torso attachment from the part that bends with the arm. Rotating the whole sleeve with the upper arm can pull the shoulder seam away from the torso. Likewise, attach a sock to the appropriate lower-leg surface rather than blindly inheriting shoe rotation.
- Check local joint angles and the resulting world-space direction together. A plausible hand or shoe rotation in isolation can become an excessive wrist or ankle bend after parent rotation.
- Fix limb trajectory and parent-joint motion before forcing an endpoint with extreme wrist/ankle rotation. Weights do not correct a wrong pose.
- Maintain joint volume without exposed circular caps or bulges. Inspect inner and outer contours at rest, mid-motion, and contact, including the return path; good contact and start poses can hide an unnatural lowering motion.

## 4. Choose a Deformation Method for Each Part

Official: [Bones](https://rive.app/docs/editor/manipulating-shapes/bones), [Meshes](https://rive.app/docs/editor/manipulating-shapes/meshes).

| Required result | Method | Misconception to avoid |
| --- | --- | --- |
| Move or rotate a part while preserving its shape | Group/Bone parenting | Becoming a bone's child does not make a shape bend |
| Bend part of a vector smoothly | Bind and weight path vertices and Bézier handles | Do not start by generating an image mesh for a vector |
| Bend raster cloth, hair, or body surfaces | Generate an image mesh, then bind and weight it | Rotating a flat image does not replace joint deformation |
| Control multiple poses through a single control | Add a Joystick to the required rig | A Joystick does not create good intermediate poses on its own |

- Production judgment: Mix methods within a rig, such as rigid hands and shoes with weighted sleeves.
- Choose based on the asset type and deformation requirements. Adding bones or meshes to every part is not the goal.

## 5. Weights and Seams Between Overlapping Parts

Official: [Bones](https://rive.app/docs/editor/manipulating-shapes/bones). For binding order, joint weight solves, options, and tool limits, follow [MCP rigging operations](mcp-operations.md#rigging-tools).

- Weights represent bone influence on a vertex and sum to 100%. Successful binding and natural deformation are separate results.
- For overlapping parts that form a continuous surface, inspect whether their shared region deforms together through bending. A correct neutral overlap can still open a seam when weights differ.
- After changing weights, inspect the joint and neighboring surfaces for folding, tearing, and volume loss; broader blending is not automatically better.
- Output: a bone-to-target map and rendered test poses showing the joint and neighboring surfaces.

## 6. Repair Mesh Structure

Official: [Meshes](https://rive.app/docs/editor/manipulating-shapes/meshes), [Intro to meshes](https://rive.app/blog/intro-to-meshes).

- Editing the contour changes structure without deforming the image. Moving mesh vertices deforms pixels in the connected triangles.
- Content outside the contour is hidden. Check the contour and transparent margins before changing weights to address clipping.
- Consider interior vertices or forced edges only where needed around joints, cloth folds, or important patterns.
- Production judgment: Test a low-density mesh first and refine only where needed. Excessive density makes weight editing and diagnosis harder.
- If automatic mesh generation cannot produce the required structure, check which edits the available UI supports.

## 7. Choose FK, IK, and Constraints

Official: [IK Constraint](https://rive.app/docs/editor/constraints/ik-constraint), [Transform Constraint](https://rive.app/docs/editor/constraints/transform-constraint).

- FK creates poses through bone rotations. IK calculates a chain's rotations to reach an endpoint target.
- Consider IK for planted feet or hands touching objects, and FK for free arm swings.
- Check the IK Bone Count, Invert Direction, and Strength. If multiple constraints affect the same bone, also check their order.
- A Transform Constraint copies transforms independently of the hierarchy. Check the source and destination's local/world spaces.
- Production judgment: IK is not an automatic walk generator. Pelvis movement, foot trajectories, and contact timing require motion design.
- While a foot target should remain planted on the scene floor, check that it does not move with the character root.
- If a target exceeds the chain's reach or the knee bends in the wrong direction, first correct the target path, chain, and bend direction.

## 8. Reuse Poses with Joysticks

Official: [Joysticks](https://rive.app/docs/editor/manipulating-shapes/joysticks).

- A Joystick scrubs timelines assigned to its X and Y axes. It suits reusable controls such as head direction, gaze, and hand poses.
- Distinguish Handle X/Y from the Joystick's own Position on the stage. Pose control usually uses the Handle.
- If both axis timelines control the same property, they can conflict. Establish property ownership and the intended mixing first.
- Production judgment: Test the center, axis endpoints, and all four corners. Good endpoint poses alone do not guarantee natural intermediate blends.

## 9. Extreme Poses and Diagnostic Order

| Symptom | Check first | Recheck after the fix |
| --- | --- | --- |
| A joint opens a gap | Overlap allowance, pivot, shared weighting for a continuous surface bound to the same bones | Maximum bend and poses in the opposite direction |
| Body or clothing patterns flatten | Weight range, influence from nearby bones, mesh structure | Silhouette and volume in neutral and extreme poses |
| Feet slide | Target space, contact interval, root/background speed | Actual playback with the floor and shadows visible |
| A knee pops or bends backward | IK direction, chain count, target reach | The full target trajectory |
| Hands or props overlap incorrectly | Hierarchy and draw order | Poses immediately before and after the crossing |
| Moving a control has no effect | Timeline or constraint conflicts on the same property | The control alone and in combination |

- Structure check: query target IDs and types, hierarchy, bindings, weights, constraints, and controlled properties.
- Visual check: inspect actual renders of neutral, expected maximum bend, maximum extension, opposite-direction poses, and control combinations.
- Contact and scene check: play the motion through its range with the background, contact objects, and shadows to inspect contact, occlusion, and style preservation.
- Check editor support and [Draw Order Rules](https://rive.app/docs/editor/animate-mode/animating-draw-order). Do not unnecessarily break the hierarchy.
- Successful queries provide structural evidence. Do not record visual verification as complete without observing the actual pixels, deformation, and playback.
