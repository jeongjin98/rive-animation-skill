# Object Motion Families

Use after the [shared motion invariants](artwork-rigging.md#shared-motion-invariants). Classify the requested movement rather than the object's name. These are production judgments: examples identify reusable structures, not required objects or simulations. Combine families only where the action needs them.

## Choose the family

| Movement family | Parts and controls | Invariant and representative checks |
| --- | --- | --- |
| Rigid translation / path following | One rigid assembly with separate travel and optional orientation controls. Examples: a moving block, vehicle body, floating icon. | Internal distances stay fixed. Turn along the path only when intended; inspect starts, stops, corners, and inherited movement. |
| Pivot rotation / continuous spin | Separate support from rotating part; put the pivot at the hinge or axle. Examples: hinged panels, levers, rotors. | Support stays attached; preserve clearance and radius. Check extremes, reversal, and angle interpolation across full revolutions. |
| Rolling | Coordinate travel with rotation around the contact shape. Examples: wheels, cylinders, rolling balls. | For a circular profile rolling without slip, distance equals radius × angle in radians; use effective visible radius and consistent units. Check floor-relative speed, contact height, and shadows. Noncircular shapes need changing contact geometry. |
| Linked articulation / sliding / telescoping | Separate rigid links, hinges, tracks, and sliders. Examples: folding assemblies, articulated arms, extending mechanisms. | Preserve link lengths, endpoint connections, track axes, and enclosure overlap. Check full extension, folding, unreachable targets, and part collision. |
| Swing / pendulum / suspended motion | Separate the anchor, connector, and hanging mass; distinguish a rigid rod from a flexible cord. | Anchor follows its support; connector stays joined and retains intended length. Check turnarounds, lag after support movement, and settling rather than uniform-speed reversal. |
| Flexible bending / trailing / waving | Keep attachment regions stable; use deformable paths/meshes or a chain for free sections. Examples: ribbons, cables, flags, stems. | No tearing at the base, sharp kinks, or texture collapse. Delay free ends relative to the driving motion; one identical sine wave on all parts gives no traveling bend. |
| Compression / stretch / rebound | Separate force/contact controls from deformable mass. Examples: springs, cushions, soft blobs. | Keep anchors and contacts aligned; preserve the design's volume response. Do not squash rigid fittings. Check compression, release, overshoot, and rest shape. |
| Flight / drop / impact / bounce | Separate trajectory, orientation, collision shape, and optional impact deformation. | Contact timing drives compression/rebound. Inspect penetration, acceleration, diminishing bounces when energy is lost, and final rest; rotation alone must not change the contact height accidentally. |
| Internal flow / slosh / fill | Separate container, contents, and containment mask. Examples: liquid, granular fill, flowing indicators. | Contents stay inside except at intended outlets; preserve quantity unless filling/emptying. For gravity-driven liquid use a roughly world-horizontal surface with delayed slosh. See [liquid guidance](motion-recipes.md#drinking-contact-swallowing-and-liquid). |

## Compose and verify

Describe the dependency in a short chain, for example: travel → rolling wheels; moving support → swing → flexible trailing end; container tilt → liquid redistribution. Solve the driving motion and attachment/contact first, then add secondary response. Do not double-apply motion through the hierarchy and a constraint.

Separate rigid sections from flexible connectors even within one object. Choose transforms, bones, vector deformation, or raster meshes using [Artwork and rigging](artwork-rigging.md#4-choose-a-deformation-method-for-each-part). A motion family does not imply a particular Rive feature or automatic physics.

For keyed fixed actions, inspect the intended extremes and returns. For interactive motion, also test the allowed input range and interruptions; record any dependence on a fixed trajectory. Label authored approximations accurately instead of claiming a physical simulation.
