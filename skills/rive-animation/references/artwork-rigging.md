# Artwork Preparation and Rigging Decisions

Reference date: 2026-09-06. This guide draws on the official Rive documentation and the `mesh_rigging_tool` schema available on that date.
This is a production decision guide. Its examples are not assets verified through creation, export, and playback.
`Official` identifies documented features, `Tool` identifies observed API capabilities, and `Production judgment` identifies practical recommendations.
If menus or tools have changed, read the current documentation and call schema. Do not invent unverified arguments or operations.

## 1. Prepare Artwork for Rigging

- Production judgment: First identify the reference artwork, final display size, moving parts, and hidden areas.
- Prepare surfaces that movement will reveal, such as the torso behind a raised arm or the background behind separated legs.
- When composition, color, silhouette, or texture matters, compare the imported still image with the original before rigging.
- Do not split static backgrounds and props into unnecessary pieces. Separate only what the motion requires.
- Output: a still image, part list, style characteristics to preserve, and required range of motion.

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

## 5. Observed `mesh_rigging_tool` Call Sequence

Tool: Before use, check the live call schema, target IDs and types, and existing bindings. Prefer verified IDs over names.

1. Vector path: use `bindBones` directly. `generateMesh` is unnecessary.
2. Raster image: use `generateMesh` to create a deformable mesh, then apply `bindBones` to that target.
3. Initial binding assigns automatic weights. Do not repeat `autoWeight` with the same settings by habit.
4. Call `autoWeight` when settings or bone configurations change, or when overlapping parts need to be solved together.
5. Use `querySkin` to inspect bound bones and a weight summary. Request per-vertex values only when needed.

- Verify the `imageId` or `imageName` for `generateMesh` in the current file. Do not guess a target when no valid one is available.
- At the reference date, `trace` defaults to true. Set it to false only when the user requests a plain rectangular mesh.
- `detail` controls contour tracing; `subdivisions` controls interior density. Do not start with maximum density.
- This tool does not support bone creation, bone animation, or per-vertex weight painting.
- Check whether other available tools support the required operation in their current schemas. Do not invent calls from tool names.
- Use UI alternatives through the UI tool's documented API and observed screen state. Do not substitute unsupported internal access.

## 6. Weights and Seams Between Overlapping Parts

Official: [Bones](https://rive.app/docs/editor/manipulating-shapes/bones). The shared weighting rule below comes from the observed tool description.

- Weights represent bone influence on a vertex and sum to 100%. Successful binding and natural deformation are separate results.
- When overlapping parts bound to the same bones form a continuous joint surface, pass them together as `targetIds` in one `autoWeight` call.
- The tool solves them as a single surface. Separate solves can produce different weights in the overlap and open a seam when the joint bends.
- If targets have different bone sets, first confirm how they should connect. Do not include unrelated parts in a shared solve.
- `blend` ranges from 0 to 1. Lower values are more rigid; higher values blend more broadly across boundaries. Higher is not always better.
- `maxInfluences` allows up to 4 influences per vertex. Retest the joint and nearby parts after changing `smooth` or `blend`.
- The official editor provides individual weight adjustment, locking, and Smooth. Do not assume this MCP tool exposes the same detailed controls.
- Output: a bone-to-target map and test poses. Correct weight totals do not establish that folding, tearing, or volume loss is acceptable; inspect the render.

## 7. Repair Mesh Structure

Official: [Meshes](https://rive.app/docs/editor/manipulating-shapes/meshes), [Intro to meshes](https://rive.app/blog/intro-to-meshes).

- Editing the contour changes structure without deforming the image. Moving mesh vertices deforms pixels in the connected triangles.
- Content outside the contour is hidden. Check the contour and transparent margins before changing weights to address clipping.
- Consider interior vertices or forced edges only where needed around joints, cloth folds, or important patterns.
- Production judgment: Test a low-density mesh first and refine only where needed. Excessive density makes weight editing and diagnosis harder.
- If automatic mesh generation cannot produce the required structure, check which edits the available UI supports.

## 8. Choose FK, IK, and Constraints

Official: [IK Constraint](https://rive.app/docs/editor/constraints/ik-constraint), [Transform Constraint](https://rive.app/docs/editor/constraints/transform-constraint).

- FK creates poses through bone rotations. IK calculates a chain's rotations to reach an endpoint target.
- Consider IK for planted feet or hands touching objects, and FK for free arm swings.
- Check the IK Bone Count, Invert Direction, and Strength. If multiple constraints affect the same bone, also check their order.
- A Transform Constraint copies transforms independently of the hierarchy. Check the source and destination's local/world spaces.
- Production judgment: IK is not an automatic walk generator. Pelvis movement, foot trajectories, and contact timing require motion design.
- While a foot target should remain planted on the scene floor, check that it does not move with the character root.
- If a target exceeds the chain's reach or the knee bends in the wrong direction, first correct the target path, chain, and bend direction.

## 9. Reuse Poses with Joysticks

Official: [Joysticks](https://rive.app/docs/editor/manipulating-shapes/joysticks).

- A Joystick scrubs timelines assigned to its X and Y axes. It suits reusable controls such as head direction, gaze, and hand poses.
- Distinguish Handle X/Y from the Joystick's own Position on the stage. Pose control usually uses the Handle.
- If both axis timelines control the same property, they can conflict. Establish property ownership and the intended mixing first.
- Production judgment: Test the center, axis endpoints, and all four corners. Good endpoint poses alone do not guarantee natural intermediate blends.

## 10. Extreme Poses and Diagnostic Order

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
