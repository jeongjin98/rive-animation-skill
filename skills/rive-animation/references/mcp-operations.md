# Rive MCP operations

Read this reference when automating the editor. These operational notes come from connected tool descriptions checked on 2026-09-06. The schema available at execution time takes precedence. Tool names below are capability discovery hints; do not construct arguments from a name alone. Availability and naming may differ across integrations.

## Inspect, change, inspect again

1. Use `session_info` to obtain the open and active file IDs/URLs, then `list_artboards` to identify the intended artboard. The selected artboard and sticky active state may differ.
2. Start with a shallow `get_artboard_hierarchy` query and narrow to the relevant branches. Obtain object IDs through `find_objects` or `query_objects`. Disambiguate identical names by ID.
3. Inspect timelines, state machines, keys, View Models, and script diagnostics as needed. Capture important existing values and the actual visual baseline before editing.
4. Check types, units, and enums in the relevant tool definition and `query_property_keys`; read existing values with `query_property_values`.
5. Apply a small set of changes with one purpose. Check returned IDs and per-property errors, then query the same targets again.
6. Inspect poses and playback after structural changes. Independent reads may run in parallel; edits that share the active document must run sequentially.

When UI interaction is needed, reacquire the current app or tab through the host's available UI tools and follow their documented API. Do not reuse stale tab handles or guessed coordinates. Verify a rename through actual file information.

## Properties and units

| Operation | What to verify |
|---|---|
| `set_property_values` and UI-style keyframe numbers | The observed tool uses percentage-style values for opacity, scale, trim, and constraint strength, and degrees for rotation. In this setter, 100% opacity is `100`. Inspect the property's allowed range; a percentage unit alone does not imply every property is capped at 100. |
| `mesh_rigging_tool.autoWeight` | `blend` is a separate 0–1 value; `maxInfluences` allows up to 4. Do not convert every number to a percentage. |
| Enums | Check the enum choices and accepted values in `query_property_keys`. Do not guess a display name or integer key. |
| Error results | Check individual errors such as `unknown_id`, `type_mismatch`, `invalid_enum_value`, and `read_only`. A returned response does not mean every change succeeded. |

Keep runtime API units separate from editor MCP units. Recheck the schema and queried values for other versions.

## Timelines, state machines, and assets

- Inspect the default `Timeline` with `listLinearAnimations` and `queryKeyFrames`. If it still has the default name and no keys, reuse and rename it for the first animation. Inspect the default state machine's connections too.
- Reuse or modify existing names, keys, and graphs within the request. Do not overwrite a timeline already in use for unrelated work. Add a timeline when an additional animation needs one.
- Use `createStates` to add states to an existing layer. Avoid unnecessary layers or state machines.
- In the observed tool, the earlier key controls interpolation for the following interval. Query keys and interpolation after changing them.
- `upload_asset` registers an asset in Assets. Placing it on the canvas requires a separate image or SVG instance. Verify registration and placement separately.

## Rigging tools

- For raster deformation, identify the image, then use `generateMesh` followed by `bindBones`. A vector path can use `bindBones` directly without mesh generation.
- Initial binding also calculates automatic weights. Use `autoWeight` when settings or bones change, or when overlapping surfaces need to be recalculated together.
- **When overlapping images or vectors form one joint surface and are bound to the same bones, include them together in one `autoWeight` call's `targetIds`.** Solving them separately can produce different weights in the overlap and a visible tear during bending. Do not combine unrelated parts.
- Inspect the binding with `querySkin` and verify a rendered maximum bend. Request detailed vertex weights only for diagnosis.
- This mesh tool does not create bones, animate them, or paint individual weights. Discover another available tool for those operations, or use supported editor UI controls.
- Follow [Artwork and rigging](artwork-rigging.md) for artwork choices and further pose tests.

## Listeners and data

- The observed `create_listeners` requires a target. A listener without a target is not exported to the runtime.
- For artboard-wide pointer interaction, use an appropriate hit-target shape covering the intended area. Distinguish pointer targets from artboard event or View Model targets.
- Design hover as enter/exit and press as down/up, with cancellation recovery where needed. Connect View Model changes to transition conditions.
- `addComponentList` creates instances from a View Model list. Do not manually add the same items inside it again.
- For new runtime notifications, check current mechanisms such as View Model triggers. Discovering a legacy feature does not authorize migrating the whole project.

## Simulation and export

`simulateStateMachine` reports state entry, transitions, events, and resting states; **it does not render pixels**. Running 240 frames or reaching the expected state does not visually verify joints, illustration fidelity, or a loop.

- Inspect the graph and listener/script behavior before simulation. Restoring property values does not necessarily undo side effects of executed listener actions.
- The observed tool may reject a run while a timeline or state machine is open for editing. Resolve the editor state through a supported UI/API path.
- Default unconditional transitions also count. Compare runs with and without the intended input to verify that it drives the desired state.
- Check normal-speed editor playback and, when required, the actual exported `.riv` separately. Follow [Verification and handoff](verification.md).

`export_file` can export the current in-memory document, including unsaved changes. Reconfirm the exact file and provide an existing destination directory. A `.riv` is a runtime export; a `.rev` is a backup containing editor information. Verify the actual returned path.

Hosted assets or asset inclusion in a `.rev` may require network access. Respect stated plan restrictions; do not use another format to bypass them. Check that the exported file opens and includes the expected assets.

For session errors, reacquire current state and tools, then use a supported recovery path. Do not repeat the same failing operation without a relevant state change. Finish independent work and report the blocked operation and remaining verification.

Official overview: [Rive MCP](https://rive.app/docs/editor/ai/mcp). Detailed argument, unit, and rigging notes above are based on connected tool descriptions from the stated review date, not a bundled or pinned MCP implementation.
