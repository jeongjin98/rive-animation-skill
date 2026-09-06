# Interaction and Runtime

Read this when building assets that respond to data or preparing them for app integration. This design reference is based on documentation reviewed on 2026-09-06. Check the target platform's current documentation for exact APIs and feature support.

## Data contracts

For external control in new work, consider View Models and Data Binding first. Check the legacy guidance for existing State Machine Inputs and runtime Rive Events. For a small change that preserves existing behavior, assess only the necessary scope; do not add a product-wide migration.

[Data Binding](https://rive.app/docs/editor/data-binding/overview), [Inputs guidance](https://rive.app/docs/editor/state-machine/inputs), [Migration Guide](https://rive.app/docs/editor/data-binding/migration-guide)

For each property, record its name, type, initial value, unit or range, owner of changes, and direction of data flow. Also define how to handle delayed initial data, cancellation, retries, repeated input, and the connection to the View Model instance.

Example contract for a progress indicator; match the actual names to the app:

| Property | Type and initial value | Meaning and direction |
|---|---|---|
| progress | number, 0 | App → Rive, 0–1; converted to a displayed percentage or trim value |
| isBusy | boolean, false | App → Rive, operation in progress |
| activate | trigger | Rive → app, a user request to start; the app owns the actual operation result |

Keep number, boolean, and trigger properties distinct. A View Model value of `progress=0.5` and an MCP opacity value of `50` follow different contracts.

## State graphs and property ownership

- Define timelines and states, transition conditions, the initial state reached from Entry, the return path after completion, and the state after interruption.
- For a single loop, use the existing simple connection. For multiple UI states, choose states that correspond to user actions, such as Idle, Pressed, Busy, and Success.
- Check blend durations and exit conditions. Verify that repeated input, moving the pointer away while pressed, cancellation, and re-entry cannot leave the state machine stuck.
- Multiple layers, component animations, direct bindings, and scripts can conflict when they write to the same property. Assign a primary control path to each property and inspect mixing and transitions.
- Check listener targets and data changes. After logical simulation, inspect the actual hit areas and responses at normal playback speed.

[State Machines](https://rive.app/docs/editor/state-machine/state-machine), [Transitions](https://rive.app/docs/editor/state-machine/transitions), [Layers](https://rive.app/docs/editor/state-machine/layers), [Listeners](https://rive.app/docs/editor/state-machine/listeners)

## Components, Layout, text, and audio

- Group repeated visual elements or rigs into Components and expose only the necessary data. Do not complicate a scene used once merely to make it reusable.
- Check which timeline or state machine runs inside each component and how it mixes with other animations. Also check the current export and component settings for artboards the runtime needs to access.
- For responsive UI, define Layout size, padding, gap, alignment, and child sizing. For a fixed illustration, decide the composition and fit or crop first.
- Check dynamic text for length, wrapping, overflow, and font glyph coverage. Test aspect ratios, initial values, and missing assets when swapping images or components.
- When audio is needed, check cue timing, looping and interruption, muting, and the platform's playback requirements.

[Components](https://rive.app/docs/editor/fundamentals/components), [Layouts](https://rive.app/docs/editor/layouts/layouts-overview), [Text](https://rive.app/docs/editor/text/text-overview), [Fonts](https://rive.app/docs/editor/text/fonts), [Audio](https://rive.app/docs/editor/assets/audio)

## Choosing Scripting

MCP tools that control the editor and Luau Scripting that runs inside an asset serve different roles. The skill supplies instructions; it does not configure an MCP connection or provide tools. Keep poses and timing editable through native keys and rigs. Consider scripting for mathematical transformations, procedural behavior, and custom drawing, layout, or effects.

- Choose the narrowest protocol that fits the purpose. Check current support for Node, Layout, Converter, Path Effect, Transition Condition, Listener Action, Test, and any other protocol you need.
- Read types and APIs through the reference tools available in the current integration, such as `get_scripting_reference` when provided. Check what file creation and editing tools are responsible for, and do not invent functions.
- Use typed inputs and the existing View Model contract. Assess whether per-frame computation is necessary, and handle null or delayed data and re-entry.
- Check diagnostics, recompile when needed, then inspect console output from an actual execution. Existing console output is not evidence that the new code ran.
- Use meaningful Test scripts for reusable calculations or state logic. Inspect visual results separately.
- Introduce WGSL or shaders only when the request needs them and the target renderer and runtime support them.

[Scripting](https://rive.app/docs/scripting/getting-started), [Protocols](https://rive.app/docs/scripting/protocols/overview), [Debug Panel](https://rive.app/docs/scripting/debugging/debug-panel)

## App integration and performance

Identify the target platform, SDK version, and renderer from the existing code or specification. If these are unknown, continue work that does not depend on them while asking for the missing information. Do not report support for a platform you have not verified.

1. Check the features in use against [Feature Support](https://rive.app/docs/feature-support) and the [runtime documentation](https://rive.app/docs/runtimes/getting-started). Successfully loading a file in an older SDK does not establish support for newer features.
2. At the actual display size, inspect image resolution, vector vertex counts, mesh density, and the number of active animations. Look for excessive vertices in automatically traced vectors and oversized raster images.
3. Define which assets are embedded or loaded externally and the required font glyph coverage. Verify that the actual export includes the necessary artboards, components, and assets. Do not combine export rules from different versions by assumption.
4. Do not keep a static waiting state running through an unnecessary permanent loop or blend. Handle playback outside the visible area, pause and resume, and reduced motion according to the use context.
5. Apply default values, extreme values, and repeated input to the actual `.riv` file. Identify rendering, response, and resource-use bottlenecks on the target device before optimizing.

[Best Practices](https://rive.app/docs/getting-started/best-practices), [Reduced Motion](https://rive.app/docs/editor/accessibility/reduced-motion), [Semantics](https://rive.app/docs/editor/accessibility/semantics)

The handoff contract includes the file or editor URL; artboard, state machine, and View Model names; property initial values, types, and directions; assets; display size and fit; runtime and renderer; and verification results. Do not automatically include an SDK migration or deployment that was not requested.
