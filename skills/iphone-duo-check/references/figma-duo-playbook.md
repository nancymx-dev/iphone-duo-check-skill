# Figma Duo playbook (use_figma)

Practical notes for Step 4. Load the Figma plugin's `figma-use` skill first, if it's available.

## Ground rules

- **Build copies, never edit the original.** `orig.clone()`, append to the same parent, place in a row to the right (`x = prev.x + prev.width + ~200`). Check the parent section's other children so you don't overlap them. Name each `<original> – Duo <pose> – Option <X>`.
- **Pose sizes, guides and device frames come from the iOS and iPadOS 27 kit** (`ios27-duo-kit.md`). Never invent dimensions. If the file already has `iPhone Duo` device instances, read their variant and size first and match them.
- **Don't touch fills, strokes or colour variables.** For new text, `clone()` an existing text node and change `characters`; for new controls, clone existing instances.
- **Fonts:** load the fonts of nodes you edit before changing characters. Check the styles in use with `getStyledTextSegments(['fontName'])`.
- `figma.currentPage` resets each call: walk `node.parent` up to `PAGE` and `await figma.setCurrentPageAsync(page)`.

## Pose artboard (built from the kit)

Each pose gets two things on the page, at the same x,y, to the right of the original:

```
<original> – Duo <pose> – Option <X>                 copy of the user's frame, resized to the pose; carries the layout grids
<original> – Duo <pose> – Option <X> – Device frame  locked frame holding only the kit's iPhone Duo device instance
```

Keep the screen copy on the page rather than moving it into a wrapper: files often use font styles that aren't installed, and moving text then fails.

1. **Screen copy:** `clone()` the user's frame, rename it, move it, `resize()` to the pose size, `clipsContent = true`. On rail poses (outer, inner landscape), hide the copy's `Status bar` (the system draws it in the rail), set the content container's right padding to its original value + 84, and give it the top padding it used to get from the status bar, from the existing spacing scale.
2. **Layout grids** on the screen copy (values from `ios27-duo-kit.md`). These replace coloured guide layers completely:
   - Side rail (outer, inner landscape): `COLUMNS`, `alignment: 'MAX'`, `count: 1`, `sectionSize: 84`, `offset: 0`, neutral grey at about 15% alpha
   - Fold, inner portrait folded: `ROWS`, `STRETCH`, `count: 2`, `gutterSize: 40`, `offset: 0` (the gutter lands on y 456 → 496)
   - Fold, inner landscape folded: `COLUMNS`, `STRETCH`, `count: 2`, `gutterSize: 40`, `offset: 0` (the gutter lands on x 456 → 496)
   - Content margin: `COLUMNS`, `STRETCH`, `count: 1`, `offset` = the design's side padding, very low alpha
   - At most two grids per screen, so the view stays readable.
3. **Device frame (optional):** import the pose's Standard template by key, create a temporary instance off to the side, `detachInstance()`, move only its `iPhone Duo` child into a new frame (no fill, `clipsContent = false`) at the pose position and the template's offset (outer portrait −29,−27; inner −40,−40), lock it, then delete the rest of the temporary copy. If the import fails, skip the device frame and say nothing.

**Never put Apple's guide components on the canvas** (Layout Margin, Safe Area Inset, Reserved Region, Fold Avoidance Region). Their solid colours cover the design, and designers don't want them in the file. Read their positions from the kit reference instead.

Don't add Apple's own toolbar or tab bar to the user's design either: they're Apple's controls, not the app's. If an option moves the app's tab bar to a side rail, mock it from the app's own tab items (see "Side rail mock").

## Inspect without blowing the 20KB return limit

Design system components often carry many hidden slot layers. List the root's direct children (id, name, visible, size, visible text joined with `" | "`), then dump one representative section skipping hidden nodes:

```js
function d(x, depth){
  if (!x.visible) return [];
  let s = `${'  '.repeat(depth)}${x.type} "${x.name}" ${x.id} ${Math.round(x.width)}x${Math.round(x.height)}`;
  if ('layoutMode' in x && x.layoutMode!=='NONE') s+=` AL:${x.layoutMode} gap${x.itemSpacing} pad${[x.paddingTop,x.paddingRight,x.paddingBottom,x.paddingLeft]}`;
  if ('layoutSizingHorizontal' in x) s+=` sz:${x.layoutSizingHorizontal}/${x.layoutSizingVertical}`;
  if ('maxWidth' in x && x.maxWidth) s+=` max${x.maxWidth}`;
  if (x.type==='TEXT') s+=` "${x.characters.slice(0,40)}" fs${x.fontSize}`;
  const out=[s];
  if ('children' in x && depth<7) for (const c of x.children) out.push(...d(c,depth+1));
  return out;
}
```

Look for `FIXED` widths and heights: they're the usual reason a screen won't stretch to a Duo pose. Instances count too, e.g. a page header component left at a fixed 351pt overflows the 334pt outer-display column; set it to `FILL`.

## Recipes

**Resize a copy to a pose:** `copy.resize(w, h)` with the kit's size, then make top-level content `layoutSizingHorizontal='FILL'`. Keep content inside the pose's content area: on outer and inner-landscape poses, add right padding equal to the 84pt rail (it's a safe area inset) instead of moving content by hand. Scrolling content: `layoutSizingVertical='HUG'` inside the Screen frame, which keeps the pose height with `clipsContent=true`.

**Check the fold:** for folded poses, compare each interactive node's absolute bounds with the fold band (inner portrait y 456 → 496; inner landscape x 456 → 496, relative to the Screen frame). Return any overlaps by name.

**Max content width on inner display:** wrap the content column in an auto-layout frame (`FILL` width) with `counterAxisAlignItems='CENTER'`, and give the column `maxWidth` from the existing design's largest sensible width. Ask if there's no obvious value.

**Even-column grid:** parent `layoutMode='HORIZONTAL'`, `layoutWrap='WRAP'`, children `FILL` with equal widths; confirm the gap lands on the centre line.

**Keep an element off the fold:** move it into the left or right half's container rather than nudging x; absolute nudges break when sizes change.

**Side rail mock (no design system component yet):** a vertical auto-layout frame, 84pt wide, on the pose's right edge (matching the kit's `Vertical Bar`), built from clones of the existing tab bar items (icon + label). Order: Back/Close at top, then the main action, then tabs. Name it `Side rail (mock)`.

**List + detail:** clone the detail screen's content into a second pane beside the list in a horizontal auto-layout (`FILL`/`FILL`); add a selected state using the existing selected row style only.

## Gotchas hit before

| Symptom | Cause | Fix |
|---|---|---|
| `in appendChild: unloaded font "<Family> Regular"` when moving the screen copy into a wrapper | The file uses font styles that aren't installed (e.g. a `Regular` or `SemiBold` style where the installed family only has `Medium` and `Semibold`) | Don't move the copy: keep it on the page (see "Pose artboard"). Load fonts with `Promise.allSettled` so a missing style doesn't abort the script |
| Designer finds coloured guides distracting | Apple's guide components were placed on the canvas | Remove them; use layout grids only (see "Pose artboard") |
| Inner portrait: Done sits in a cyan bottom band | The template's 95pt bottom inset is Apple's tab bar, not a system inset | Only a finding if the screen has its own tab bar; otherwise mention it once and leave it |
| `HUG can only be set on auto-layout frames` | Plain frame holding children | Set `layoutMode='VERTICAL'`, `itemSpacing=0`, padding 0 first, then HUG |
| Container keeps old size | Sizing mode didn't recompute | Also set `primaryAxisSizingMode='AUTO'` and re-set the sizing on children |
| `findOne(n => n.parent === x)` null inside instances | Object identity differs in instance sublayers | Match by name or compare `.id` |
| New auto-layout frame clips text | `createAutoLayout` frames start FIXED | After `appendChild`, set FILL / HUG and `clipsContent=false` |
| Text runs off a wide frame | Text is `WIDTH_AND_HEIGHT` | `layoutSizingHorizontal='FILL'` + `textAutoResize='HEIGHT'` |

## Verify

`await frame.screenshot({scale:1})` after each step. On every artboard, look for: anything clipped, anything under the fold band, camera reserved region or side rail, single lines stretched across the whole inner display, controls missing compared with the other poses, and frames that didn't resize. Fix before continuing; failed scripts are atomic, so a fixed retry is safe.
