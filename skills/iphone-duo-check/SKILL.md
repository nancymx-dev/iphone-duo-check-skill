---
name: iphone-duo-check
description: Guide a designer step by step through checking and fixing an app screen for iPhone Duo (Apple's two-display foldable) — collect the screen, check it pose by pose (outer display, inner portrait, inner landscape, half-folded, Split View) against Apple's "Designing for iPhone Duo" HIG, recommend layout options in a published artifact, let the user pick one, then build pose artboards in their Figma file from Apple's iOS and iPadOS 27 kit (Duo templates, guides, fold region, device frames) with permission. Use this whenever the user mentions iPhone Duo, iPhone Fold, foldable iPhone, the fold or hinge, inner or outer display, dual displays, device poses, half-folded, book pose, vertical tab bars or side controls, reserved regions, or asks whether a design "works on the Duo" or "on the foldable", even if they don't say "skill" or name the HIG.
---

# iPhone Duo check: poses to Figma update

This skill walks a designer through four gated steps. Each step ends by asking the user something, so don't run ahead: the value of the flow is that the designer stays in control of which option ships and when their file is touched.

```
1. Collect   → the screen, which surface, load the guidance
2. Recommend → artifact with pose-by-pose check + options, user picks one
3. Target    → Figma link to the exact frame, permission to edit
4. Update    → build pose artboards on copies, verify, report
```

Track progress with a short step marker in your replies (e.g. "Step 2 of 4") so the user always knows where they are.

## How to ask: short and concise

Designers skim, so every question to the user should be one or two short lines and ask for one thing. Leave out anything they don't need to answer the current question: no explanations of the guidance, no tool or connector details, no restating the plan, no lists of what you checked. Put reasoning and detail in the artifact; offer more in chat only if they ask. Use these scripts as the default wording (adapt the bracketed parts):

**Step 1**
> **Step 1 of 4: the screen**
> Please share the screen you want checked: a Figma link (right-click the frame → Copy link to selection) or a PNG.
> Is it for the **native app** or the **webapp on mobile**?
>
> Blur any real customer data first.

**Step 1 → 2 (after analysis)**
> Got it. Checking each pose now.

(The pose-by-pose findings go in the artifact, not the chat.)

**Step 2**
> **Step 2 of 4: pick an option**
> Results and options are here: [artifact link]. I recommend **[X]**: [reason in ≤ 10 words].
> Which one should I use?

(Then the AskUserQuestion with one choice per option.)

**Step 3: link** (skip if they already gave a frame link in Step 1)
> **Step 3 of 4: Figma link**
> Paste the link to the frame (right-click the frame → Copy link to selection).

**Step 3: confirm frame + permission**
> That's "[frame name]" ([w×h]). I'll build the Duo pose artboards as copies next to it and leave the original and colours unchanged.
> OK to go ahead?

**Step 4: done**
> **Step 4 of 4: done**
> Option [X] is on [n] Duo artboards, to the right of "[name]". Press Control + G to show or hide the rail and fold grids.
> Not done: [only items that need a decision, one line each].
> Want the full change list or the questions for engineering?

Only include "Not done" if there's something to decide. Give the change list and engineering questions only if they say yes.

If something blocks a step (missing screen, Figma not signed in, unknown display size), say what's needed in one line and how to get it in one more.

## The core principle: let it resize, don't redesign it

Apple's own rule for the Duo is "don't reinvent your app when it resizes; allow the existing layout to expand". That matches the designer's interest: the screen is already designed and reviewed. A Duo check that comes back with a new design forces them to re-argue settled decisions.

So every option, and every artboard in Step 4, keeps:
- the same content, copy, order and information hierarchy
- the same components, visual style, colours, type styles, icons, radii and dividers
- the same functionality and state in every pose (nothing available on the inner display goes missing on the outer one, and a half-filled form survives folding or unfolding)

and changes only what's needed for the screen to fit each pose: auto-layout behaviour (fill, hug, wrap, max-width), how content spreads across a wider frame, keeping tappable things out of the fold, and where controls sit. Spacing stays on the existing scale.

The one deliberate exception Apple encourages: on the large inner display you **may** show one extra level of hierarchy side by side (e.g. a list + its detail, like Mail). Treat that as a "Minor addition" or "Interaction change" option, never the default, and only when the content naturally has a list and a detail.

Rank options from the smallest change to the largest, and recommend the smallest one that actually fixes the findings. Apple also says to avoid extreme layout changes as people fold the device: favour small moves over rearrangement.

## Source of truth

`references/hig-iphone-duo.md` is a condensed summary of Apple's HIG page **Designing for iPhone Duo** (https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo, new page dated 9 September 2026). Read it at the start of every run. Every finding and option should trace back to it.

The live page renders with JavaScript, so a plain web fetch returns nothing. To check for updates, download its data feed with curl into a scratch folder:
`https://developer.apple.com/tutorials/data/design/human-interface-guidelines/designing-for-iphone-duo.json`
and read the "Change log" table at the end of `primaryContentSections`. If there's an entry newer than the one in the reference file, use the live content, tell the user it changed, and offer to refresh the summary.

### Apple's Figma kit: iOS and iPadOS 27

**Required Figma setup:** Before using this skill, add the **iOS and iPadOS 27 UI Kit** by **Apple Design Resources** in Figma and enable its library in the design file you want checked. Confirm that its components are available in that file before starting. If it is missing, ask the user to add and enable it before continuing.

`references/ios27-duo-kit.md` lists the iPhone Duo templates, guides and device frames in Apple's **iOS and iPadOS 27** community library, with component keys, exact sizes and where the side rail, camera and fold sit in every pose. Read it at the start of every run. It's the only source for artboard sizes, fold position and margins, and every mock and artboard is built from it. If the kit has been updated (check with `search_design_system`, query `iPhone Duo`, for new or renamed templates), trust the live library over the reference file and tell the user.

If the user's team has its own design system guidance for iPhone Duo or foldables (in a wiki, docs tool or design system site the session can reach), search for it once. Where it's more specific than Apple's, prefer it and tell the user.

### Things the HIG doesn't settle

- **Display sizes.** The HIG gives no point dimensions; the iOS and iPadOS 27 kit does (outer 466 × 678, inner 669 × 951, plus landscape and folded versions). Never guess numbers, and never use sizes from anywhere else.
- **Custom navigation.** Apple says standard system bars move to the side automatically. Apps built with a custom tab bar or toolbar (common in React Native, Flutter or a custom design system) probably don't get that for free. Ask the user whether their app uses system bars; if not, flag vertical-control findings as "needs engineering" rather than treating them as a design fix only.
- **The design system's breakpoints.** Ask the user (or read their design system or code) where the layout switches from phone to tablet. A common value is 768pt: then outer (466 / 678) and inner portrait (669) get the **phone** layout, and inner landscape (951) gets the **tablet** layout. So rotating the open device swaps layouts: check both, and ask engineering to confirm the app sees these sizes as-is. If the user doesn't know the breakpoint, assume 768 and say so.
- **The webapp.** In Safari, the browser owns its own bars; the webapp mostly sees a viewport that changes width. Check outer, inner and Split View widths; mark fold-region and vertical-control checks as "native only" unless engineering confirms the web gets fold information.

## Step 1: Collect

Ask for the screen (Figma frame link preferred, PNG fine) and whether it's native app or webapp. If they have Duo screenshots from Xcode Device Hub or a test device, take those too, but don't require them: the default iPhone design is enough to check.

Remind the user to crop or blur anything that looks like real customer data, and use test data in mocks. If something you receive appears to contain customer PII (names, email addresses, phone numbers), point it out and suggest a redacted version. The user can override if they judge it acceptable.

If you get a Figma link, screenshot the frame yourself and read its auto-layout (see the playbook's inspect script) so findings are based on real sizing, not guesses from pixels. Confirm that the required iOS and iPadOS 27 UI Kit library is enabled in the user's design file before running the check.

Run the check against the kit's geometry, not general impressions: lay the screen's content against each pose's content area, side rail, camera reserved region and fold avoidance band (`references/ios27-duo-kit.md`, "Geometry of each pose"). For example, a CTA at y 470 on inner portrait sits in the fold band (y 456 → 496) when folded.

Then check the screen in each pose from `references/duo-patterns.md` ("The poses to check"), giving each a verdict:
- **Pass:** fits with no change
- **Risk:** will probably work, but depends on something unconfirmed (display width, how a custom tab bar behaves)
- **Fail:** content or functionality lost, overlapping the fold or a reserved region, or stretched badly

Tie each Risk or Fail to a cause, e.g. "Inner, half-folded: the 'Continue' button sits centred across the fold. It's a fixed-width button centred in the frame."

## Step 2: Recommend options and ask for a choice

Read `references/duo-patterns.md` for the pattern catalogue and pick 2–4 options that fit **this** screen and **these** findings, ordered from least to most change. Don't pad with options that don't solve anything.

Choose the presentation surface for the assistant environment:
- **Claude:** use an Artifact for the pose check and layout options. Follow any available Artifact guidance; do not require a separate `artifact-design` skill unless the environment explicitly requires it.
- **OpenAI environments, including ChatGPT and Codex:** use Sites for the pose check and layout options. Load the available Sites skill and follow its supported creation and publishing workflow. Keep the Site private by default; do not make it public without permission.
- If the selected surface is unavailable, explain the limitation and present the same findings and options in chat. Do not claim to have created an Artifact or Site, and do not silently switch to a different publishing service.

Use “Artifact” or “Site” in the Step 2 message to match the surface actually created, and link to the result. The choice of surface does not change the option-selection or Figma-editing permission gates.

Include:
- a short **recommendation** at the top: which option and why, in one or two sentences
- a **Pose check** panel: one row per pose with its verdict and cause
- the **options** side by side. For each:
  - **Title:** a letter plus a short name (e.g. "B. Fold-safe actions")
  - **Mini mocks** of the screen in the poses it changes (at least outer portrait and inner open; add folded if the option touches the fold), drawn to the kit's proportions (outer 466 × 678, inner 669 × 951 or 951 × 669). Draw the fold as a subtle dashed band where the kit puts it (across the screen in inner portrait, down it in inner landscape), the 84pt side control rail and the camera region where they apply. Match the user's design: real copy, same colours, weights and icons. The only visible difference should be how the layout fits each pose.
  - **Change size:** "Layout only", "Minor addition" or "Interaction change"
  - **What changes:** 3–6 concrete bullets
  - **Works well:** 2–3 bullets
  - **Watch for:** 2–3 honest trade-offs or risks
  - **Needs engineering:** anything design can't settle alone (custom tab bar placement, breakpoint, reserved-region API)
  - **Guidance link:** which HIG rule(s) it satisfies
- a short **Questions for engineering** list at the end (only real open questions)

Then ask which option to go with, using AskUserQuestion with one choice per option (recommended first, marked "(Recommended)"). Accept combinations. The chosen option's "What changes" list is the spec Step 4 implements.

## Step 3: Target and permission

Ask for the **direct Figma link to the frame** (a URL with `node-id=`), unless Step 1 already gave one.

Check the Figma connector is signed in (the `use_figma` tool must be available). If it isn't, tell them to type `/mcp` in the message box, choose the Figma server and sign in, or use the Connectors menu under the **+** button. Don't send them to Figma's Dev Mode MCP setting: that server is read-only.

Confirm that Apple's iOS and iPadOS 27 UI Kit has been added in Figma and its library is enabled in the target file. Use its components and the geometry in `references/ios27-duo-kit.md`. If the components cannot be accessed, explain the blocker and ask the user to check the library setup before editing.

Screenshot the linked node, confirm it's the right frame, then ask for permission in one line (see the script). By default you'll build copies and make **no colour, fill, stroke, token, type style, icon or copy changes, and no restyling** beyond the spec's minimal additions.

Wait for a clear yes. If the user wants you to edit the original in place, confirm that separately.

## Step 4: Update the design

Load the `figma:figma-use` skill (it's a hard prerequisite for `use_figma`), then read `references/figma-duo-playbook.md`.

Work in small calls:
1. Inspect the node tree shallowly (returns cap at 20KB).
2. Make a row of pose artboards to the right of the original, one per pose the option covers, named `<original name> – Duo <pose> – Option <X>`. Size each from the kit template, and put a copy of the user's design inside it (see the playbook's "Pose artboard" recipe).
3. Fix structure in each copy so it fits the pose's content area (fill and hug, max-widths, wrap).
4. Apply the option's changes per pose (fold-safe actions, extra hierarchy, control placement).
5. Show the pose's side rail, margin and fold as **Figma layout grids** on each screen copy, never as coloured layers. Designers toggle layout grids with Control + G, and grids never appear in exports or prototypes. Add the matching `iPhone Duo` device frame from the kit as a separate locked layer; if the kit does not import, resolve the library access blocker before continuing. Never place Apple's coloured guide components (Layout Margin, Safe Area Inset, Reserved Region, Fold Avoidance Region) on the canvas: they cover the design's own colours.
6. Screenshot after each meaningful step and fix anything clipped, stretched or sitting in the fold before moving on.

Keep colours intact by reusing what exists: clone existing nodes and instances. If the spec needs something that doesn't exist (e.g. a vertical tab rail component), stop and say so: mock it from existing parts, label it "mock, needs a design system component", and list it under "Not done".

Finish with the short Step 4 script. Keep the full change list and the engineering questions ready, and share them only if asked.

Don't list next steps unprompted. If the user asks what's next: build the other options, run the same check on related screens in the flow, check large text sizes on the outer display, or run an accessibility check.

## Tone

The user is a product designer. Use design language (frame, auto layout, fill, hug, wrap, token, artboard) and no code jargon. When you must mention an engineering detail (breakpoint, safe area), say it in plain words once. Keep every message short: lead with the step marker, give only what's needed, end with one question, and don't move on until it's answered.
