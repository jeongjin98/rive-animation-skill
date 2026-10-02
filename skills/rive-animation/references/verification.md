# Verification and Handoff

Read this when evaluating completion and inspecting visual quality. Apply only the checks relevant to the request. This is not a procedure for requesting user approval again at every stage.

## Criteria and evidence

| What to assess | Valid evidence | Insufficient on its own |
|---|---|---|
| File and structure | Current ID or URL; inspection of artboards, objects, keys, and states | An old filename or tab handle |
| State transitions | The current graph and state or transition results from executing input | Key counts or the number of default unconditional transitions |
| Illustration style, joints, timing, and loops | Poses and continuous playback rendered by Rive with the current changes applied | Structural JSON, simulation traces, or a source preview from before the changes |
| Export | Returned path, file existence and size, and opening the file in the intended format | Clicking Export or checking the extension |
| App behavior | Loading the `.riv` in the target runtime and renderer, then checking inputs, sizing, and playback | Success only in the editor or playback in a different SDK |
| Integration contract and lifecycle | Confirming the intended names/types/instances, then exercising affected loading, failure, repeat-input, and teardown/remount paths in the host app | A parsed schema or cleanup code that was never executed |

Structural, visual, and runtime checks complement one another. Passing one does not make another pass. The skill provides verification instructions; the current environment must separately provide the tools needed to inspect rendered playback.

For nonvisual integration fixes, apply the affected [contract and lifecycle checks](interaction-runtime.md#initialization-failures-and-ownership). Report code/configuration inspection separately from executed app behavior; do not require a new style reference or claim a visual re-review when no visual authoring occurred.

## Task-specific acceptance checks

Before visual edits, derive a short set of checks from the requested action and inspectable reference. Record `expected behavior or preserved trait / observable failure signal / poses or intervals to inspect` in the brief or existing part map. Include the qualities the request depends on, such as silhouette, rhythm, weight, or expression, as well as the relevant [shared relationships](artwork-rigging.md#shared-motion-invariants). For a small correction, add only the defect-specific check and affected dependencies; reuse existing criteria.

Examples to adapt, not requirements for every style:

| Requested result | Observable pass condition | Inspect |
|---|---|---|
| A jump with a weighted landing | The intended contact, compression, and recovery phases read in sequence; planted feet retain contact until intentional release | Landing through recovery at normal speed; contact-change frames |
| A drinking action | Grip and mouth contact hold during the sip; lowering preserves the shoulder seam and a continuous wrist trajectory | Raise, sip, and lower, including the return transition |
| Preserve a reference expression and rhythm | Specified eye/mouth shapes and silhouette remain recognizable; the intended holds and accelerations remain distinct | Matched reference/current poses and full-speed action |

Replace labels such as “natural” or “subtle” with the visible behavior intended for this task. Preserve intentional stylization; use numerical tolerances only when the brief or a technical constraint supplies a meaningful one. These criteria guide the agent's checks and do not create a new user-approval stage.

## Match claims to the implementation

- Before delivery, compare the agreed method with actual object types, hierarchy, bindings, and keys. Record any deviation from a proposed or promised method.
- Claim bone weighting only after inspecting actual bones and bound targets/weights. Group parenting alone is a rigid rig; a script that draws a character is not automatically an editable bone/mesh rig.
- Separate implementation failure from capability limits. Inspect available tools before saying Rive cannot perform an operation that the current asset simply does not implement.
- “Playback works” establishes execution, “joints remain connected” establishes continuity, and “motion looks natural” needs inspection of pose, anatomy, force, timing, and intermediate transitions. Key counts and matching loop endpoints cannot substitute for these checks.

## Final spatial and structural consistency pass

Before handoff of visual work, perform this pass against the task-specific checks and the five shared relationships: attachment, occlusion, shape/volume, contact/separation, and scale/depth. Apply the relevant [human](human-motion.md), [object](object-motion.md), or [animal](animal-motion.md) checks within this pass; they do not require duplicate playback runs.

1. **Compare the current result with the baseline.** Match composition, scale, framing, and pose/time where comparable. Check the specified silhouette, color, linework, texture, expression, and background. Inspect assembled subjects and the full scene for [relative size and perspective](artwork-rigging.md#scene-scale-and-perspective); isolated parts cannot establish scene coherence.
2. **Inspect poses and transitions.** Check neutral and maximum-deformation poses, intermediate frames in both movement directions, and immediately before/during/after changes in contact, direction, visibility, or depth. Look for gaps, detachment, unintended movement, texture stretching, volume loss, clipping, and penetration. Use enlarged views, slow playback, or scrubbing for diagnosis. Temporarily hide occluders or use a contrasting background when needed, then restore all inspection-only changes before full-scene playback and export. For corrections, compare before/after at matching visibility so a prop cannot conceal the defect.
3. **Play the complete action at delivery size and normal speed.** Assess the expected rhythm, weight, trajectories, anticipation, and follow-through. For loops, watch consecutive cycles and both half-cycles, including boundary position, velocity, occlusion, tiles, and shadows. For one-shots, inspect start, completion, and replay; include requested entry/exit and state transitions. Judge scale, contact, and depth throughout travel. Do not judge timing from stills.
4. **Exercise applicable interaction and delivery paths.** Test repeated/rapid input, interruption, pointer exit, extreme data values, and re-entry where relevant. Inspect current editor playback first; if export or actual use is in scope, also play the exported `.riv` and perform the required target-runtime checks. Footage from another revision or an arbitrary SVG rendering is not evidence of this result.
5. **Repair and recheck.** Identify the failed criterion and cause before changing hierarchy/pivots, trajectories, draw order/coverage, deformation/bindings, or scene placement. Change a distinguishable cause at a time, then replay the affected interval and complete action with normal layers restored. Recheck dependent fixes after later edits, such as grip and liquid after wrist changes or clothing after reparenting; do not repeat unrelated checks without a reason. Update the verification record with the affected parts/interval and observed result.

Intentional releases, turns, crossings, and depth changes can alter relationships. They must follow the intended action without unexplained jumps, flashes, detachment, or penetration. A far hand may emerge beyond a garment silhouette but must remain hidden while behind opaque clothing. Do not hide, shrink, or fade parts to conceal an error; legitimate clipping follows the intended occluding boundary.

Any unresolved unintended relationship failure or unmet task-specific acceptance check is a failure of the relevant visual check, even if the animation plays successfully. A still image can reveal a defect but cannot establish its cause or behavior over time.

## Investigation by symptom

| Symptom | Inspect first | Possible correction |
|---|---|---|
| Joint gaps | Pivots, parent transforms, overlap, draw order, and shared bone weights | Correct the hierarchy or pivots; calculate weights together for overlapping surfaces influenced by the same bones |
| Far hand/limb appears through clothing or flashes during a crossing | Intended occluder, draw order, limb trajectory, coverage, and deformation before/during/after the event | Apply the occlusion criterion in the [final consistency pass](#final-spatial-and-structural-consistency-pass); repair the cause and replay the full cycle |
| Person looks giant beside a vehicle or building | Relative dimensions at comparable depth, import bounds, assembly scale, ground contacts, and camera/projection | Correct scene scale or depth placement coherently; recheck contacts, shadows, and the full travel path |
| Sliding feet | Ground contact, root motion, environment speed, and foot trajectories | Align the coordinate relationships and walking or environment speeds |
| Whole-body wobble | Duplicate transforms and keys with identical phase | Separate the primary motion, reduce amplitude, and delay follow-through |
| A jump at the loop boundary | First and last values, velocity, draw order, and tile seams | Correct interpolation, boundary velocity, occlusion, or tile continuity |
| Different behavior in the app | SDK and renderer, export, listener targets, and external assets | Check support and contracts; re-export the correct file and verify it again |
| A stuck state | Initial instance, conditions, and conflicts between layers, mixing, and scripts | Correct ownership of changes, initial values, or the return path |

Confirm the cause before changing the asset. Do not hide it by adding keys, subdividing the mesh, or changing opacity.

## Deliverables and reporting

- **Editable source:** The actual editor URL and a `.rev` backup when needed. Preserve the rigs, timelines, and parts that need to remain editable.
- **Runtime file:** The app's `.riv`, together with any external image, font, or audio dependencies.
- **Preview:** A capture or video of actual playback when requested or useful for review. Identify the file, revision, and inspection environment.
- **Integration contract:** The applicable [runtime handoff fields](interaction-runtime.md#app-integration-and-performance), including preserved or intentionally changed public names, data/instance ownership, failure behavior, cleanup, and unverified scope.

A `.rev` is an editable backup; a `.riv` is a runtime deliverable. After export, verify that the file exists and is usable. Share publicly or deploy only when that is within the requested scope.

[Backup Export](https://rive.app/docs/editor/exporting/exporting-for-backup), [Runtime Export](https://rive.app/docs/editor/exporting/exporting-for-runtime)

## Verification record

Every production or fix handoff must identify the inspected file/URL and revision (or saved snapshot), plus the inspection environment. Record each required check as `check / status / observed evidence / remaining issue or next check`. Cover the applicable structural, visual, export, and runtime checks, including task-specific acceptance checks. Concise bullets or a table are sufficient; no separate report file or video is required unless requested or needed to substantiate a claim.

- **PASS:** The check was performed on the identified result and its acceptance condition was met. Name the observed pose/interval, input/result, comparison, or export-open result; “checked” alone is insufficient.
- **FAIL:** An observed result violates the criterion. Identify the defect and affected scope; another passing check does not override it.
- **UNVERIFIED:** A required check was not performed or the available evidence cannot establish its result. State the missing access/evidence and next check. An inaccessible renderer is not a pass or an exclusion.
- **N/A:** The check is outside the requested scope. Give the reason when omitting a category that could otherwise be expected, such as runtime testing for an editor-only deliverable. Do not enumerate unrelated tests.

Declare a requested scope complete only when all its required checks pass. Partial handoff is allowed: distinguish completed scope from failed or unverified scope. Planning and read-only reviews report findings and evidence limits, without claiming new production or inventing verification results.

If access or export is blocked, confirm the current session and use supported recovery paths. If the same cause keeps producing failures and no new recovery path is available, finish independent work, then report the blocked operation/cause and the file/check to resume. Keep known defects as FAIL even when further playback becomes unavailable; mark any outstanding recheck UNVERIFIED.
