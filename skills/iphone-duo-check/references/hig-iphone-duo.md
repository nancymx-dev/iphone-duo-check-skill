# Designing for iPhone Duo: HIG summary

Condensed summary of Apple's Human Interface Guidelines page, written for design checks. Not a copy: go to the source for exact wording.

- Source: https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo
- Data feed (for update checks): https://developer.apple.com/tutorials/data/design/human-interface-guidelines/designing-for-iphone-duo.json
- Change log at time of summary: **9 September 2026, new page**. Summarised October 2026.
- Related: Apple Design Resources (Duo template, margins, safe areas), HIG "Layout", "Designing for iOS", "Split views", "Toolbars"; developer doc "Preparing your app for iPhone Duo".

## The device

- Two displays, each with its own front camera, and a centre hinge.
- **Outer display:** used when closed. Wider and shorter than other iPhones, so the system puts toolbars and tab bars **on the side** to save vertical space. Outer camera sits in a corner, always visible, lined up with the side controls.
- **Inner display:** used when open. Camera hidden behind the display until active.
- It's still an iPhone: all iOS patterns still apply.

## Poses

- Held like a book (partly folded), laid flat, or standing on its edges.
- **Don't design a custom layout per pose.** Use size classes: **compact width for the outer display, regular width for the inner display**. That covers every pose. Let the existing layout expand into available space instead of reinventing it.
- Xcode Device Hub previews the Duo in its poses.

## Best practices

| Rule | What it means for a design check |
|---|---|
| Build to resize | Many sizes are possible (two displays, poses, Split View). Use layout margins and safe areas; avoid fixed widths and anything tied to one display. |
| Consistent across displays | Same functionality and same state on both displays. Keep the hierarchy; the inner display may show **one extra level** side by side (Mail: list or message when closed, both when open). |
| Same functionality in every pose | Controls may overflow and content may move, but everything stays reachable. |
| Follow the system's vertical controls | Toolbars, tab bars and nav controls go on the side on the outer display and on the inner display in landscape. |
| Games | Fill the screen in every pose; keep text and control sizes steady; prefer changing aspect ratio over letterboxing. |

## Dynamic layouts and reserved regions

Reserved regions are areas content avoids or components adapt to (like iPad window controls):
- **Outer front camera:** always present; grows into the Dynamic Island for Live Activities.
- **Inner front camera:** only when the camera is active; the UI moves aside.
- **Folding region:** only when partly open. Splits the inner display into usable regions and excludes the centre strip.

System alerts, context menus and sheets move away from the fold automatically; split views adjust column widths and margins to the inner display's symmetry. Custom components need the ReservedRegion API (engineering).

- **Adapt when the device folds.** Prefer containers that adapt automatically (Notes' split view panes). In grids, **prefer an even number of columns** so content divides cleanly at the fold. Keep important elements clear of the centre.
- **Avoid extreme layout changes when folding.** Move only what's needed to keep things visible and tappable; small adjustments beat rearranging.

## Split views and arrangement views

- Split view: expands on the inner display, collapses to one pane on the outer display (like regular vs compact on other iPhones).
- **Arrangement view:** a container with a primary and secondary view, arranged by size, orientation and reserved regions.
  - *Split* arrangement: side by side when wider than tall, stacked when taller than wide.
  - *Overlay* arrangement: one on top of the other; when partly folded, they move to each side.
- Use one when the layout already looks like one (side by side or stacked maps to split; layered maps to overlay).
- Keep navigation (tab views, navigation split views) **outside** the arrangement.

## Vertical controls

- On the side: Dynamic Island, status bar, toolbar (including nav buttons) and tab bar. **Exception: inner display in portrait keeps normal horizontal bars.**
- In Split View multitasking on the inner display, each app puts its controls on its **outer edge** (left app on the left).
- Side controls stay aligned with the hardware, including in right-to-left languages.

Rules:
- **Account for asymmetry.** Controls on one edge make content space lopsided; use safe areas so controls (including the other app's in Split View) don't cover content.
- **Keep controls consistent across poses.** Keep relative positions similar so people don't relearn where actions are.
- **Placement order:** top of the side rail = primary navigation (Back, Close), then prominent actions (Done). Keep other toolbar groupings; the system separates top-bar and bottom-bar items.
- **Prioritise frequent items.** Items overflow bottom to top by default; set visibility priority so frequent actions (Compose, New Note) and status items with badges stay visible longest.
- **Don't override default bar placement** in general; side controls are a core Duo pattern.
- **Full width is OK where bars aren't needed** (immersive, non-scrolling, like Calculator), as long as nothing clashes with the Dynamic Island or status bar. A full-width header or background with inset scrolling content is also fine.
- **Group toolbar items** rather than spacing them by hand.
- **Keep controls near the content they affect** (Mail's list controls stay above the list pane, not on the side rail).
- **Every non-text toolbar item gets a title and a symbol.** The title is used in overflow menus and expanded forms.
- **Few text-only buttons.** Text labels keep a bar horizontal; prefer symbols.
- **When space is tight:** navigation-focused screens keep the tab bar and push toolbar items to overflow (default); task-focused screens minimise the tab bar to keep the task's toolbar actions.
- **Use the system overflow menu.** Merge any custom overflow into it; reserve the ellipsis symbol for overflow.

## Not in the HIG page

- No point dimensions for either display (see Apple Design Resources).
- Nothing specific to web content in Safari.
