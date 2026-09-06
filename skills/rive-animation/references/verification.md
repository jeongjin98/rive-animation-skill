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

Structural, visual, and runtime checks complement one another. Passing one does not make another pass. The skill provides verification instructions; the current environment must separately provide the tools needed to inspect rendered playback.

## Visual inspection

1. Compare the reference and the current artwork in the same composition. Check silhouette, color, linework, texture, expression, and background.
2. Inspect key poses and maximum deformation for joint gaps, unintended movement of parts, stretched textures, occlusion, and clipping.
3. Play at the intended size and normal speed. Inspect weight, ground contact, trajectories, anticipation, follow-through, and excessive oscillation. Do not judge timing from still captures.
4. Watch loops several times, including the boundary. Check continuity of position, velocity, occlusion, background tiles, and shadows. For one-shot animations, inspect start, completion, and replay.
5. Test interactions with repeated or rapid input, interruption, the pointer leaving its target, extreme data values, and re-entry. Do not add conditions that do not apply.
6. Make changes in groups with distinguishable causes, then recheck the affected poses or intervals. Do not repeat unrelated checks that already passed without a reason.

Inspect playback in the current editor first. If export or actual use is in scope, also play the exported `.riv` file. Do not present footage from another file or an earlier revision, or an arbitrary SVG rendering, as evidence of the current result.

## Investigation by symptom

| Symptom | Inspect first | Possible correction |
|---|---|---|
| Joint gaps | Pivots, parent transforms, overlap, draw order, and shared bone weights | Correct the hierarchy or pivots; calculate weights together for overlapping surfaces influenced by the same bones |
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
- **Integration contract:** Required artboards, state machines, View Models, properties, initial values, units, directions, runtime and renderer, and any unverified scope.

A `.rev` is an editable backup; a `.riv` is a runtime deliverable. After export, verify that the file exists and is usable. Share publicly or deploy only when that is within the requested scope.

[Backup Export](https://rive.app/docs/editor/exporting/exporting-for-backup), [Runtime Export](https://rive.app/docs/editor/exporting/exporting-for-runtime)

Example report format, not a claim that this work has been completed:

> Updated the walk and the bag's follow-through. Checked joints, ground contact, and the loop boundary in representative poses and repeated playback of the current file. The editable source and `.riv` are included. Playback in the target app remains unverified because its SDK information was not available.

Report only checks that were actually performed. If access or export is blocked, confirm the current session and use supported recovery paths. If the same cause keeps producing failures and no new recovery path is available, finish work that can proceed independently, then record `completed scope / remaining work / blocked operation and cause / file to inspect next`. Do not mark animation quality as complete while visual inspection remains unverified.
