# iPhone Duo Check Skill

A design-assistance skill for checking an existing app screen across outer, inner, folded and Split View layouts, comparing options, and updating copies in Figma with permission.

**Collect → Recommend → Choose → Update and review**

## Start here

Read [Getting started](skills/iphone-duo-check/GETTING-STARTED.md). The installable folder is `skills/iphone-duo-check/`.

## Required Figma setup

**Required Figma setup:** Before using this skill, add the **iOS and iPadOS 27 UI Kit** by **Apple Design Resources** in Figma and enable its library in the design file you want checked. Confirm that its components are available in that file before starting.

## Install

Download the single-skill ZIP from the GitHub release, extract it, and copy the entire `iphone-duo-check` folder into your assistant's supported skills directory. Reload skills as required by your assistant.

Alternatively, copy `skills/iphone-duo-check/` from this repository. Keep its `references/` folder alongside `SKILL.md`.

## Try it

> Use iphone-duo-check to check this screen across Duo poses. This is a native app. Compare layout options and ask me to choose before making any Figma changes.

You can also specify that the screen is a mobile webapp. Share a frame link or a redacted screenshot.

## Requirements and limitations

- An assistant able to read skill instructions and inspect images.
- An editable Figma connection for direct design changes. The instructions use Figma plugin tools and related skills that may vary between assistants.
- **Claude:** the pose check and layout options are presented in an Artifact.
- **OpenAI environments, including ChatGPT and Codex:** the pose check and layout options are presented using Sites, private by default. Public sharing requires permission.
- These presentation tools are provided by your assistant environment, not bundled with this skill. If the appropriate tool is unavailable, the assistant explains the limitation and presents the findings and options in chat.
- The bundled references record device geometry and Apple guidance as supplied with this skill. Their source dates, component keys and availability need verification against the live sources before relying on them.

This is an independent project, not an Apple product or endorsement. No Apple design assets or customer screens are bundled.

## Included

- `SKILL.md`: the four-stage workflow and permission gates.
- `references/hig-iphone-duo.md`: guidance summary and source links.
- `references/ios27-duo-kit.md`: recorded pose geometry and component identifiers.
- `references/duo-patterns.md`: layout patterns and trade-offs.
- `references/figma-duo-playbook.md`: Figma editing and verification recipes.

## License

[MIT](LICENSE). Created by [Nancy](https://github.com/nancymx-dev). External sources and assets remain subject to their own terms.
