# iPhone Duo pattern catalogue

Patterns for making an existing design fit iPhone Duo without redesigning it. Within each component they run from **least change** to **most change**. Start at the top and only go further when the lighter pattern can't fix the finding.

Change size labels used in the artifact:
- **Layout only:** auto-layout and sizing behaviour; nothing visible is added or removed
- **Minor addition:** one small, justified change using existing components (an extra pane on the inner display, a title added to an icon button, an item moved out of the fold)
- **Interaction change:** the screen behaves differently (e.g. tab bar becomes a side rail, two screens merge into list + detail). Last resort, and usually needs engineering.

## The poses to check

Check every screen in these, in this order. "Native only" means skip or mark as Risk for the webapp.

| # | Pose | What's different | Common failures |
|---|---|---|---|
| 1 | **Outer display, portrait (closed)** · 466 × 678 | Compact width; 84pt control rail on the right; camera region top right (84 × 169); content area ~362 wide, which is *narrower* than a 402pt iPhone | 402/375 layouts that assume their width; content too tall for 678 (CTA pushed off the first view); custom tab bar still at the bottom eating height (native only); content under the camera or rail |
| 2 | **Inner display, portrait** · 669 × 951 | Regular width, but still the **phone** layout if the tablet breakpoint is 768; horizontal top bar and bottom tab bar | Phone layout stretched to 629pt content width (very long lines, huge full-width buttons, lonely single column) |
| 3 | **Inner display, landscape** · 951 × 669 | The **tablet** layout if the tablet breakpoint is 768; 84pt rail on the right (native only) | Layout jumps to tablet only when rotated; rail makes content lopsided; tablet layout never designed for a 669 height |
| 4 | **Folded (book or tent)** | 40pt fold avoidance band: **across** inner portrait at y 456 → 496, **down** inner landscape at x 456 → 496 (native only) | Buttons, inputs, keypads, sliders, chart midpoints or key numbers sitting in the band; odd-column grids with a column on the fold; a card split between halves |
| 5 | **Split View on inner display** | Each app about half the inner width; controls on its outer edge | Assumes full width; content covered by the side controls of either app |
| 6 | **Fold / unfold mid-task** | Layout switches between compact and regular while in use | Lost form input or step; scroll position reset; a control that existed on one display missing on the other |

Pose 6 is a flow check, not an artboard: note it as a finding and test it in the prototype.

## Universal fixes (apply to almost every option)

The baseline; most "Layout only" options are just these:
- Fixed-width frames → **fill width**, with a **max content width** for text-heavy content so lines don't stretch across the inner display.
- Fixed heights → hug, so the outer display's short height scrolls instead of clipping.
- Use layout margins and safe areas, not hard-coded offsets.
- Keep spacing, type, icon sizes and touch targets the same as the default design; the Duo changes space, not scale.
- Keep every control and piece of content reachable in every pose (overflow is fine, removal isn't).

## Forms and single-task flows (sign-up, onboarding, profile editing, booking)

| Option | Change size | What changes | Watch for |
|---|---|---|---|
| A. Fill with max width | Layout only | Form column fills on outer; centred with max width on inner; CTA keeps its width rule | Wide empty margins on inner; check CTA stays in reach |
| B. Fold-safe actions | Minor addition | A + primary CTA, date picker and text input sit in one half (or full width with the label clear of the centre) when half-folded | Must not move dramatically between poses |
| C. Summary beside form | Interaction change | Inner display shows the form plus a review/summary pane side by side | New layout to build; collapses back to one pane on outer |

Always note: form state must survive fold/unfold.

## Lists and detail (inbox, tasks, contacts, documents)

| Option | Change size | What changes | Watch for |
|---|---|---|---|
| A. Stretch with max width | Layout only | Rows fill; metadata stays right-aligned within a max width | Long gap between label and metadata on a wide row |
| B. List + detail on inner | Minor addition | Inner display shows the list and the selected item side by side (Apple's Mail example); outer stays list → detail | Needs a "nothing selected" state for the detail pane; selection state must match across displays |
| C. Two-column grid | Layout only | Cards/tiles in an even number of columns on inner | Odd counts leave a gap; never 3 columns across a fold |

## Dashboards, charts and summaries

- Even-column grids (2 or 4) so the fold falls between tiles (Layout only).
- Don't centre a key number, chart midpoint or interactive scrubber on the fold (Minor addition).
- Full-width hero/header with inset content is fine (HIG allows it) as long as nothing clashes with the camera.

## Navigation: tab bars, headers, toolbars (native only)

| Option | Change size | What changes | Watch for |
|---|---|---|---|
| A. Keep bars, check height | Layout only | Keep bottom tab bar; confirm content still fits the short outer display | Goes against Duo's core pattern; acceptable stopgap only |
| B. Side rail | Interaction change | Tab bar and toolbar move to the side on outer and inner-landscape; top of rail = Back/Close, then main action | A custom tab bar won't move on its own, so this needs engineering and likely a new design system component |
| C. Priority + overflow | Minor addition | Assign which toolbar actions stay visible longest; extras go to the system overflow menu | Task screens (profile editing) keep toolbar actions and minimise tabs; browse screens keep tabs |

Also: every icon-only toolbar action needs a text title (for overflow); keep controls next to the content they affect.

## Modals, sheets and alerts

- System alerts and sheets move off the fold automatically; custom design system sheets and bottom sheets may not. Flag custom ones as "needs engineering" (Minor addition).
- Bottom sheets on the inner display: max width, not edge to edge.

## Carousels, onboarding, promo tiles

- One card per half on inner when half-folded, never a card split by the fold.
- Keep paging dots and arrows out of the centre strip.

## Webapp on mobile

- Check poses 1, 2, 5 and 6 using viewport widths; mark 3 and 4 as "native only / Risk" unless engineering confirms fold info reaches the web.
- With a 768 tablet breakpoint, inner portrait (669) gets the phone layout and inner landscape (951) the tablet layout, so check both. Safari's own bars reduce the usable height.
