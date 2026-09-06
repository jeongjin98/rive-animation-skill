# Validation scope and reusable scenarios

This file documents qualitative evaluation of the original skill and provides scenarios for future changes. It is not a benchmark, a claim of cross-client execution, or evidence of a finished animation.

## Original authoring evaluation

The evaluation used one isolated no-skill baseline and three qualitative with-skill scenarios. Evaluators did not modify a real Rive file, render an animation, or run an exported asset in an app.

The baseline already handled several controls correctly: it avoided trusting stale file identity, guessing tool units, and claiming visual completion from state-machine simulation. Its observed error was to interpret a fixed camera as prohibiting all background parallax. The skill explicitly separates camera motion, character root motion, and environmental animation.

With the skill, the walk-cycle scenario retained that distinction and the other controls. The icon scenario preserved a narrow legacy-compatibility boundary and planned repeated/interrupting input. The planning scenario respected a moving-camera reference and made no file changes. These are single qualitative samples; they do not measure success rates or visual quality improvements.

The original package passed frontmatter/UI metadata validation, relative-link checks, and installed-copy consistency checks. The English distribution received a separate pre-publication review recorded below.

## Reusable scenarios

Run the same request and initial conditions in isolated contexts with and without the skill. Keep the observation criteria out of the request shown to the evaluator. Record actual responses and distinguish observed failures from hypothetical risks. For a live test, use a disposable or explicitly authorized asset and record exact versions and output identity.

### A. Continue a raster character walk

**Request:** Continue the existing file, preserve the illustration and background, use a fixed camera, create a four-second walk loop, and deliver editable source plus an app asset.

**Initial conditions:** The recorded file ID/name may be stale. Raster limb artwork overlaps at the elbow. The default timeline has not been inspected. An old state-machine trace reports 240 frames. No current playback or export has been inspected, and the target runtime is unspecified.

**Observe:**

- Reacquire current file and artboard identity before editing; preserve the existing file when asked to continue.
- Inspect existing keys before choosing to reuse or add a timeline.
- Specify camera, subject movement, and environmental animation separately.
- Choose rigid parenting, raster meshes, or vector vertex binding according to actual deformation needs.
- If overlapping surfaces are skinned to the same bones across a joint, consider a joint automatic-weight operation and inspect the bent result.
- Read the live property schema instead of guessing units. In the originally observed editor setter, 100% opacity is `100`.
- Treat simulation as logic evidence; plan actual contact, weight, joint, and loop inspection.
- Advance independent work while the platform is unspecified and qualify target-runtime verification.

### B. Data-driven interactive icon

**Request:** Make an existing icon respond to a live progress value and an activation action. Keep its colors and support repeated taps.

**Initial conditions:** The asset uses legacy state-machine Inputs and an existing keyed timeline. There is no request to migrate the whole product. The runtime version is unspecified.

**Observe:** A narrow compatibility decision, current feature-support lookup, a typed data contract with defaults/range/direction/ownership, preservation of existing keyed work, repeated and interrupted input handling, actual render checks, and honest runtime qualification. Do not require a character rig for an icon or silently migrate the whole app.

### C. Planning scope and moving camera

**Request:** Review a supplied motion reference and propose how to make it in Rive. The reference includes a moving camera. Do not edit the file yet.

**Initial conditions:** No native project data or live playback evidence is available.

**Observe:** A plan-only response that preserves the requested camera motion, reads relevant references, makes no MCP mutations, does not force a loop, and invents neither exported files nor observations of inaccessible video.

## English distribution verification

Checks performed on 2026-09-06:

| Check | Observed result |
|---|---|
| Skill frontmatter validator | Passed: `Skill is valid!` |
| YAML and optional OpenAI UI metadata | Parsed successfully; skill/folder names match; the prompt names `$rive-animation` |
| Relative Markdown links | 16 checked, none broken |
| Fresh copy layout | All seven skill files copied into three temporary client directory layouts; SHA-256 content checks matched the source |
| English and privacy review | No remaining Korean, private user paths, project file identifiers, or apparent credentials found |
| Independent English document review | Read the README, entrypoint, all references, metadata, and license; installation source paths matched the package tree |
| English scenario A | Preserved camera/root/environment distinctions, existing work, conditional joint weighting, live unit checks, and the visual/runtime evidence boundary |
| English scenario B | Kept the legacy integration within the requested scope and planned typed progress, repeat/interrupt handling, and property ownership |
| English scenario C | Produced a conditional plan with the requested moving camera; made no mutations or claims about inaccessible playback |

The independent review identified an incomplete English verification record; this section supplies the observed results. No material decision failure was found in the three English dry runs. They were hypothetical action decisions from the supplied scenarios, not live tool execution.

The temporary copy check verifies file placement and integrity only. It does not verify discovery inside the clients. No live Rive editing, playback capture, export, or target-runtime execution was performed for this release.

## Remaining evidence needed

- Run the installed English skill inside each claimed client with an actual Rive connection.
- Produce and inspect a real asset against its reference, including extreme poses and normal-speed loop playback.
- Open the editable backup and render the exported `.riv` in the intended runtime and renderer.
- Measure quality improvement only through a reproducible comparison with enough samples and a stated visual rubric.
