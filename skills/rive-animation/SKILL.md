---
name: rive-animation
description: Use when creating, continuing, improving, or reviewing Rive animation assets, character rigs, scene loops, interactive motion, or Rive production plans. File-producing work requires a concrete visual style reference before execution.
---

# Rive Animation

Create editable Rive assets that preserve the visual character of the reference. Judge quality from key poses and actual playback. Treat successful tool calls, key counts, and transition logs as evidence only of what they directly establish.

## Mandatory visual-reference gate

Do not create, modify, rig, animate, or export a Rive asset until a concrete visual style reference is available and inspectable. This is a hard precondition, not an optional quality recommendation.

- A valid reference is an attached image or screenshot, an accessible image/design URL, or an existing Rive/art asset that the user explicitly wants preserved or extended.
- A text-only description, genre label, list of colors, or phrase such as "flat vector," "cute," or "realistic" is not a visual style reference.
- An existing asset may serve as the reference only when its artwork is visible and the task is to continue, improve, or animate that same visual language.
- If no valid reference was provided, refuse the production work and ask the user to attach or link one. Do not create a draft, placeholder artwork, rig, animation, export, or improvised house style while waiting.
- If the supplied reference cannot be opened or inspected, stop and ask for a re-upload or accessible link. Do not infer its appearance from a filename or surrounding prose.
- When several references conflict, ask which is primary or state a narrow interpretation before execution. Never silently blend them into a new style.
- Planning and read-only technical advice may continue without a reference only when they do not produce or mutate an asset. Clearly label them as planning or advice.

After a reference is available, identify the concrete traits to preserve—composition, proportions, silhouette language, palette, line treatment, shading, texture, and character details—before making the first edit. Do not substitute an agent-selected art direction.

## Scope and starting point

- Follow the requested scope: creation, modification, review, or planning. Deliver a plan for planning requests and findings for read-only reviews. Do not ask again for an already agreed specification or authorization.
- Infer the purpose, existing file, duration and loop behavior, display size, action/emotion, and deliverables from context. Never infer or invent the visual style reference; enforce the mandatory visual-reference gate above before production. Proceed with small, stated assumptions only after that gate is satisfied.
- **Specify camera motion, character movement through the scene, and environmental animation separately.** A fixed camera keeps the framing and viewpoint fixed. It does not automatically freeze scenery outside a train window or stop characters moving through the scene.
- When asked to continue, verify the current file and reuse its structure. Respect requests for a new file or duplicate. Do not make a particular project, file ID, art style, or fixed camera a universal default.
- Check available capabilities before execution. This skill supplies instructions, not a Rive connection. Use an available Rive MCP integration or supported editor controls for edits and a renderer or playback viewer for visual checks. Without those capabilities, complete useful planning or review work and identify the specific execution limit.

## Choose the relevant reference

Read only the references needed for the current task.

| Task | Reference |
|---|---|
| Inspect or modify the Rive editor | [MCP operations](references/mcp-operations.md) |
| Prepare artwork, separate parts, set hierarchy/pivots, use bones/meshes/IK | [Artwork and rigging](references/artwork-rigging.md) |
| Create keyframes, walks, idles, scene loops, or refine motion | [Motion recipes](references/motion-recipes.md) |
| Connect states, data, components, responsive behavior, scripts, or an app | [Interaction and runtime](references/interaction-runtime.md) |
| Review visuals, verify a fix, export, or assess completion | [Verification and handoff](references/verification.md) |

For precise features and APIs, follow the official links in the references. Use the [official documentation index](https://rive.app/docs/llms.txt) and [Feature Support](https://rive.app/docs/feature-support) for current documentation and platform differences. References were prepared from documentation and connected tool descriptions checked on 2026-09-06. The live tool schema and target runtime documentation govern exact arguments and supported features. Use trusted HTTPS sources and respect security warnings.

## Production workflow

1. **Define the brief and visual baseline.** Record the composition, silhouette, color, line, and texture to preserve. Capture the current render of an existing asset. For a scene, include the relationship between characters and background.
2. **Inspect the current structure.** Before editing, inspect the actual file, artboard, hierarchy, timelines, rig, and data connections. Identify the specific objects and properties to change.
3. **Prepare artwork and the rig.** Prepare necessary parts, occluded areas, pivots, and draw order. Choose rigid transforms, vector vertex deformation, or raster meshes per part. Test representative extreme poses.
4. **Establish poses and timing.** Start with poses that communicate the action. Set weight, contact, and trajectories; refine interpolation; then add secondary motion for eyes, hair, clothes, and props. Handle small edits directly at the relevant stage.
5. **Connect interaction and delivery requirements.** Define data contracts early and connect state transitions once motion is stable. Keep a simple loop simple. For app assets, check supported features and intended display size during production.
6. **Play, inspect, and revise.** Review still poses and normal-speed playback. Separate structural, visual, and runtime checks. Fix the cause of a failure and replay the affected behavior. These checks are the agent's work, not mandatory user approval gates at every stage.

## Lessons from character revisions

- **Keep implementation promises traceable.** Record the chosen method per relevant part: group transforms, actual bones, weighted vector paths, raster meshes, or scripted drawing. Reconcile that choice with the implemented structure before delivery. If a proposed method cannot be completed, disclose the change and its limits; do not silently substitute group rotation for promised bone weighting.
- **Honor motion-first requests.** When the user asks for stickman → rig/motion review → artwork, preserve that order. Before final artwork, test thick proxy limbs and directional hands/feet at difficult bends; line segments cannot reveal volume, seams, or shoe orientation. Preserve accepted timing and trajectories when replacing artwork. See [Artwork and rigging](references/artwork-rigging.md).
- **Inspect what props hide.** Review the uncovered face and body silhouette before fine features. Temporarily hide occluders, then restore visibility and verify the full scene. A bottle covering a distorted face is not evidence of a correct face.
- **Verify the action, not just movement.** Apply the relevant run, swim, or drinking checks in [Motion recipes](references/motion-recipes.md). Body bobbing alone does not establish a readable action, and background/effects must respond coherently when they are in scope.
- **Convert feedback into a regression check.** Record the reported defect, its cause, the affected poses, and the observed result. Recheck previously corrected features when an edit touches their hierarchy or timing. Distinguish “plays,” “stays connected,” and “looks natural”; support each claim with the corresponding evidence in [Verification](references/verification.md).

## Completion report

Provide result links and concise observed evidence appropriate to the request. When relevant, deliver the editable source or `.rev` backup, runtime `.riv`, actual playback preview, and data contract. Export and public sharing/deployment are separate actions; follow the user's authorization for each.

If actual playback was unavailable, visual quality remains unverified. If the target platform was not run, state the runtime verification limit. Finish independent work despite access problems and record the blocked operation and remaining checks. Do not present a design recipe as a produced and verified asset.
