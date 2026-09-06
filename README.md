# Rive Animation Skill

An Agent Skill for creating, refining, and reviewing editable Rive animations: artwork preparation, rigging, motion, interaction, and visual verification.

Use it to help an AI agent preserve a reference illustration, build a character or scene animation, connect app behavior, and report what was actually verified.

**Works as a portable skill folder for Codex, Claude Code, and Cursor.** It follows the [Agent Skills format](https://agentskills.io/specification); it is not limited to Codex. Client discovery and invocation differ, and actual editing requires a separate Rive connection.

## What to expect

**This skill does not guarantee a perfect Rive animation on the first attempt.** Its main value is in refinement: it is designed to help the agent incorporate your revision requests more effectively, make targeted corrections, and preserve the parts you have already approved. Expect to review the result, give concrete feedback, and iterate.

## Quick start

1. Clone this repository or choose **Code → Download ZIP**, extract it, and open a terminal in its root folder.
2. Install the skill for one client using the instructions below.
3. For editing, open the intended file in Rive and connect the client to the editor.
4. Invoke `rive-animation` with a reference and a concrete request.

To clone with Git:

```sh
git clone https://github.com/jeongjin98/rive-animation-skill.git
cd rive-animation-skill
```

### Install for your client

These macOS/Linux shell commands install the skill for your user account. For a first installation, choose one block. If `rive-animation` is already installed at that destination, compare or back up the existing folder before replacing it. Windows users can copy the same folder with their file manager into the matching location under their user home directory.

**Codex**

```sh
mkdir -p ~/.agents/skills
cp -R skills/rive-animation ~/.agents/skills/
```

Invoke it with `$rive-animation` in Codex CLI/IDE, or select it in the desktop skill picker. Codex also provides a skill installer that can install a skill from a GitHub repository. [Codex documentation](https://learn.chatgpt.com/docs/build-skills)

Alternatively, ask Codex:

```text
Use $skill-installer to install the skill at skills/rive-animation
from https://github.com/jeongjin98/rive-animation-skill.
```

**Claude Code**

```sh
mkdir -p ~/.claude/skills
cp -R skills/rive-animation ~/.claude/skills/
```

Invoke it with `/rive-animation`. [Claude Code documentation](https://code.claude.com/docs/en/skills)

**Cursor**

```sh
mkdir -p ~/.cursor/skills
cp -R skills/rive-animation ~/.cursor/skills/
```

Type `/` in Agent chat and select `rive-animation`. [Cursor documentation](https://cursor.com/docs/skills)

For a project-only installation, copy the folder into the project's `.agents/skills/` for Codex, `.claude/skills/` for Claude Code, or `.cursor/skills/` for Cursor. Copy the entire `rive-animation` folder so its references remain available. If the skill does not appear, restart the client and check the documented skill directory.

The optional `agents/openai.yaml` file supplies OpenAI UI metadata. The shared workflow and reference files do not depend on it. Other Agent Skills clients can use the folder according to their own installation instructions; they have not been tested here.

### Connect Rive

The skill is an instruction package. It does not install Rive, configure an MCP server, provide credentials, or include a renderer.

- **Planning and review of supplied material:** an agent that can read the skill and the relevant reference material.
- **Editor automation:** a supported Rive desktop Editor and a working Rive MCP connection, or other supported editor controls available to the agent.
- **Visual verification:** access to actual playback, captured frames/video, or an appropriate Rive runtime renderer. State-machine simulation logs alone cannot establish visual quality.
- **App delivery:** the intended platform, runtime version, and an environment in which the exported `.riv` can be tested.

Follow the [official Rive MCP setup guide](https://rive.app/docs/editor/ai/mcp). As checked on 2026-09-06, the documented MCP integration runs in the Windows/macOS desktop Editor and uses the local endpoint `http://127.0.0.1:9791/mcp`. Keep the supported Rive app open and follow its current setup instructions. This is a loopback connection to your own computer, not an external website. Client configuration is separate: [Claude Code MCP](https://code.claude.com/docs/en/mcp), [Cursor MCP](https://cursor.com/docs/mcp), and your Codex MCP settings.

If editor or playback access is unavailable, the skill directs the agent to complete useful independent work and identify the remaining execution or verification limits.

## Example requests

Use the invocation syntax for your client, then add a request such as:

**Reference-based scene**

> Create a four-second looping Rive scene from this illustration. Preserve the linework, colors, and full background. Keep the camera fixed while the landscape outside the train window moves. Deliver editable source and a runtime .riv, and inspect actual playback before calling it finished.

**Continue an existing character**

> Continue the currently open character file. Improve the walk cycle without changing the illustration style. Inspect the existing rig and timelines first, fix foot sliding and joint gaps, and report which visual and runtime checks you performed.

**Interactive asset**

> Make this existing icon respond to progress from 0 to 1 and repeated activation. Keep its colors and preserve compatibility with the app's existing integration. Define the data contract and test interrupted and repeated input.

**Plan only**

> Review this motion reference and propose a Rive production plan. Preserve its moving camera. Do not edit the file yet.

## What's included

| File | Purpose |
|---|---|
| [SKILL.md](skills/rive-animation/SKILL.md) | Scope, reference routing, production workflow, and completion criteria |
| [Artwork and rigging](skills/rive-animation/references/artwork-rigging.md) | Parts, pivots, bones, vector/raster deformation, meshes, and IK |
| [Motion recipes](skills/rive-animation/references/motion-recipes.md) | Key poses, timing, walks, idles, scene motion, and an illustrative loop recipe |
| [MCP operations](skills/rive-animation/references/mcp-operations.md) | Live file identity, property units, bounded edits, and tool limitations |
| [Interaction and runtime](skills/rive-animation/references/interaction-runtime.md) | Data contracts, state machines, scripts, and platform handoff |
| [Verification and handoff](skills/rive-animation/references/verification.md) | Separate structural, logical, visual, export, and runtime evidence |

The main file routes the agent to relevant references instead of loading every guide for every request. There are no bundled animation files, executable helpers, MCP servers, or runtime dependencies.

## Validation and limitations

This is an initial instructional release. The original skill received static package checks and qualitative agent dry runs covering an existing walk cycle, an interactive icon, and a planning-only request. These checks do not establish an improvement in rendered animation quality.

Client installation paths are based on official documentation, not an end-to-end run in every client. A valid skill folder does not guarantee that a client's Rive tools or playback facilities are available. Tool names, units, export access, and feature support can change; the live schema and target runtime documentation take precedence over the dated notes.

See [validation scope and reusable scenarios](docs/validation.md) for the evidence and remaining work. The train-window example is a design recipe, not a downloadable or visually verified asset.

## Contributing

Useful contributions include reproducible failure cases, verified workflow corrections, and examples with an editable source plus actual playback evidence. Include the editor/client/runtime versions, expected behavior, observed behavior, and which checks you performed. Keep private project identifiers, credentials, and artwork without redistribution permission out of contributions.

Keep the entrypoint concise, put specialized detail in the appropriate reference, and cite official documentation for changing API or runtime behavior.

## License

[MIT](LICENSE). The license covers this repository's content. Rive and third-party tools or artwork retain their own terms. This is an independent community skill and is not an official Rive product.
