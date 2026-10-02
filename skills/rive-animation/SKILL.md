---
name: rive-animation
description: Use when creating, continuing, improving, or reviewing Rive animation assets, human or animal rigs and motion, object motion, scene loops, interactive motion, or animation handoff. Visual asset production requires a concrete visual style reference; scoped app-integration fixes can use an identified existing asset.
---

# Rive Animation

Create editable Rive assets that preserve the visual character of the reference. Judge quality from key poses and actual playback. Treat successful tool calls, key counts, and transition logs as evidence only of what they directly establish.

## Mandatory visual-reference gate

Do not create a new Rive asset or author changes to artwork, rigging, motion, or visual presentation until a concrete visual style reference is available and inspectable. This is a hard precondition for visual production, including export of newly authored visuals.

- A valid reference is an attached image or screenshot, an accessible image/design URL, or an existing Rive/art asset that the user explicitly wants preserved or extended.
- A text-only description, genre label, list of colors, or phrase such as "flat vector," "cute," or "realistic" is not a visual style reference.
- An existing asset may serve as the reference only when its artwork is visible and the task is to continue, improve, or animate that same visual language.
- If no valid reference was provided, stop the visual production work and ask the user to attach or link one. Do not create draft artwork, a rig, animation, or an export of new visuals while waiting.
- If the supplied reference cannot be opened or inspected, stop the affected visual work and ask for a re-upload or accessible link. Do not infer its appearance from a filename or surrounding prose.
- When several references conflict, ask which is primary or state a narrow interpretation before execution. Never silently blend them into a new style.
- Planning and read-only technical advice may continue without a reference. Clearly label them as planning or advice.

Nonvisual contract, binding, loading, error-handling, or lifecycle fixes for an identified existing asset may proceed without a separate style reference when they preserve the authored visuals. Inspect the existing code/file contract and use supported tools. If the fix requires choosing or changing a pose, timing, layout, color, or other visual behavior, apply the gate to that portion. Exporting an existing asset after a nonvisual fix still requires file/export checks; this exception never establishes visual quality or runtime verification.

After a reference is available, identify the concrete traits to preserve—composition, proportions, silhouette language, palette, line treatment, shading, texture, and character details—before making the first edit. Do not substitute an agent-selected art direction.

## Scope and starting point

- Follow the requested scope: creation, modification, review, or planning. Deliver a plan for planning requests and findings for read-only reviews. Do not ask again for an already agreed specification or authorization.
- Infer the purpose, existing file, duration and loop behavior, display size, action/emotion, and deliverables from context. Never infer or invent the visual style reference; enforce the gate for visual production. Use small, stated assumptions only within the applicable scope.
- **Specify camera motion, character movement through the scene, and environmental animation separately.** A fixed camera keeps the framing and viewpoint fixed. It does not automatically freeze scenery outside a train window or stop characters moving through the scene.
- When asked to continue, verify the current file and reuse its structure. Respect requests for a new file or duplicate. Do not make a particular project, file ID, art style, or fixed camera a universal default.
- Check available capabilities before execution. This skill supplies instructions, not a Rive connection. Use an available Rive MCP integration or supported editor controls for edits and a renderer or playback viewer for visual checks. Without those capabilities, complete useful planning or review work and identify the specific execution limit.

## Choose the relevant reference

Read only the references needed for the current task.

For app-integration-only work, go directly to [Interaction and runtime](references/interaction-runtime.md). Load artwork/rigging guides only if visuals change. If another Rive skill is also in use, keep this skill responsible for artwork, motion, and visual QA; coordinate one shared runtime contract instead of creating parallel schemas or repeating unrelated workflows.

For any rig or subject-motion request, first read **Shared motion invariants** in [Artwork and rigging](references/artwork-rigging.md#shared-motion-invariants), then the matching subject guide below before choosing parts or keying motion. Route by the depicted body and requested action, even when the user never says “rigging.” For mixed scenes, combine only the relevant guides; an anthropomorphic animal can use human action guidance alongside its animal-specific anatomy.

| Task | Reference |
|---|---|
| Human/humanoid action: raise an arm, bend, reach, walk, run, swim, drink | [Human parts and motion](references/human-motion.md) |
| Object action: move, rotate, roll, swing, bend, compress, unfold, flow | [Object motion families](references/object-motion.md) |
| Animal action: quadruped walk/run, jump, sit, flap, swim, slither | [Animal parts and motion](references/animal-motion.md) |
| Multiple subjects or a subject in scenery: streets, vehicles, buildings, interiors | [Scene scale and perspective](references/artwork-rigging.md#scene-scale-and-perspective) before detailed rigging or animation |
| Inspect or modify the Rive editor | [MCP operations](references/mcp-operations.md) |
| Prepare artwork, separate parts, set hierarchy/pivots, use bones/meshes/IK | [Artwork and rigging](references/artwork-rigging.md) |
| Create keyframes, walks, idles, scene loops, or refine motion | [Motion recipes](references/motion-recipes.md) |
| Connect states, data, components, responsive behavior, scripts, or an app | [Interaction and runtime](references/interaction-runtime.md) |
| Review visuals, verify a fix, export, or assess completion | [Verification and handoff](references/verification.md) |

For precise features and APIs, follow the official links in the references. Use the [official documentation index](https://rive.app/docs/llms.txt) and [Feature Support](https://rive.app/docs/feature-support) for current documentation and platform differences. References were prepared from documentation and connected tool descriptions checked on 2026-09-06. The live tool schema and target runtime documentation govern exact arguments and supported features. Use trusted HTTPS sources and respect security warnings.

## Production workflow

Apply the stages relevant to the request; a nonvisual integration fix uses structure inspection, the runtime contract, and affected integration checks without inventing a new visual brief or rig.

1. **Define the brief and visual baseline.** Record the composition, silhouette, color, line, and texture to preserve. Set [task-specific acceptance checks](references/verification.md#task-specific-acceptance-checks): expected behavior, observable failure signals, and the poses or intervals to inspect. Capture the current render of an existing asset. For a scene, establish relative subject sizes, depth, ground contacts, and camera/projection using the scene-scale guide before detailed motion.
2. **Inspect the current structure.** Before editing, inspect the actual file, artboard, hierarchy, timelines, rig, and data connections. Identify the specific objects and properties to change.
3. **Define relationships and prepare the rig.** Use the shared motion invariants to record required attachment, occlusion, shape, contact, and scale relationships, including intentional changes. Prepare necessary parts, hidden areas, pivots, and draw order; choose each part's deformation method. Test those relationships in representative poses before detailed motion.
4. **Establish poses and timing.** Start with poses that communicate the action. Set weight, contact, and trajectories; refine interpolation; then add secondary motion for eyes, hair, clothes, and props. Handle small edits directly at the relevant stage.
5. **Connect interaction and delivery requirements.** Define data contracts early and preserve existing public names and meanings. Connect state transitions once motion is stable. For app assets, apply the semantic-control, failure, and lifecycle guidance in [Interaction and runtime](references/interaction-runtime.md). Keep a simple loop simple.
6. **Play, inspect, and revise.** Perform the [final spatial and structural consistency pass](references/verification.md#final-spatial-and-structural-consistency-pass) on assembled subjects and the scene throughout motion and transitions. Separate structural, visual, and runtime checks. Fix the cause of a failure and replay the affected behavior. These checks are the agent's work, not mandatory user approval gates at every stage.

## Lessons from character revisions

- **Keep implementation promises traceable.** Record the chosen method per relevant part: group transforms, actual bones, weighted vector paths, raster meshes, or scripted drawing. Reconcile that choice with the implemented structure before delivery. If a proposed method cannot be completed, disclose the change and its limits; do not silently substitute group rotation for promised bone weighting.
- **Honor motion-first requests.** When the user asks for stickman → rig/motion review → artwork, preserve that order. Before final artwork, test thick proxy limbs and directional hands/feet at difficult bends; line segments cannot reveal volume, seams, or shoe orientation. Preserve accepted timing and trajectories when replacing artwork. See [Artwork and rigging](references/artwork-rigging.md).
- **Inspect what props hide.** Review the uncovered face and body silhouette before fine features. Temporarily hide occluders, then restore visibility and verify the full scene. A bottle covering a distorted face is not evidence of a correct face.
- **Verify the action, not just movement.** Apply the relevant run, swim, or drinking checks in [Motion recipes](references/motion-recipes.md). Body bobbing alone does not establish a readable action, and background/effects must respond coherently when they are in scope.
- **Convert feedback into a regression check.** Record the reported defect, its cause, the affected poses, and the observed result. Recheck previously corrected features when an edit touches their hierarchy or timing. Distinguish “plays,” “stays connected,” and “looks natural”; support each claim with the corresponding evidence in [Verification](references/verification.md).

## Completion report

Provide result links and the [verification record](references/verification.md#verification-record): identify the inspected file/revision and environment, and report each required check's status, observed evidence, and remaining issue. When relevant, deliver the editable source or `.rev` backup, runtime `.riv`, actual playback preview, and data contract. Export and public sharing/deployment are separate actions; follow the user's authorization for each.

Complete a requested scope only when its required checks pass. If actual playback was unavailable, visual quality remains unverified; if the target platform was not run, runtime behavior remains unverified. Finish independent work despite access problems and report completed and unverified scope separately. Planning and read-only reviews report findings and evidence limits without claiming asset production or verification.
