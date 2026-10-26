# Apple iOS and iPadOS 27 kit: iPhone Duo assets

Apple's official Figma community library **iOS and iPadOS 27** (Apple Design Resources) has ready-made iPhone Duo templates. The sizes and geometry below were read from it in October 2026 (library last updated 21 October 2026), so they apply to every file and every designer: nothing here belongs to a particular project file. Never invent sizes.

## Required kit setup and component use

**Required Figma setup:** Before using this skill, add the **iOS and iPadOS 27 UI Kit** by **Apple Design Resources** in Figma and enable its library in the design file you want checked. Confirm that its components are available in that file before starting.
- Library key: `lk-3167e7e1386e96621fc3b20782e2ec1b199754eeceff2ef04bf90d28a124be6414b67982dc8fb71ea1896b0310c316f54d7b9f0ac598cb3111748728c039a567`
- Try `figma.importComponentByKeyAsync(key)` with the keys below; importing doesn't put anything on the canvas. If it fails, ask the user to confirm that the kit has been added and its library enabled in the target file. Resolve access before editing.
- To check for a newer kit, run `search_design_system`, query `iPhone Duo`, with `includeLibraryKeys` set to the key above. If the sizes changed, trust the live library and tell the user.
- If the user's file already has `iPhone Duo` device instances, read their variant and size and match them.

## Screen templates (component keys)

| Template | Size (pt) | Key |
|---|---|---|
| Standard / Outer - Portrait | 466 × 678 | `90917f6fad609a6f4743f64a9de49354e9bb503b` |
| Standard / Outer - Landscape | 678 × 466 | `9a15af012d7d9d4ef2c9cbe4eb4c7b1350643a76` |
| Standard / Inner - Portrait | 669 × 951 | `96567a69226e5aee807547c913719a0e78c22d4e` |
| Standard / Inner - Landscape | 951 × 669 | `28f968a8b6483d552ccf90cce38925300f61ad54` |
| Standard / Inner - Portrait - Folded | 669 × 951 | `3c6145806c689767b07c6a0cb514a295c2f06b47` |
| Standard / Inner - Landscape - Folded | 951 × 669 | `2903b58d6eef6029138aacaf7eda364b03ad3fa0` |
| Sheets / Outer - Portrait | 466 × 678 | `0c4ac45edaa058df19a28c6923c455b7dbeb63bc` |
| Sheets / Outer - Landscape | 678 × 466 | `12307d2faff5f703c7fb908b209f8035822b11c9` |
| Sheets / Inner - Portrait | 669 × 951 | `af8c51d9c91dbdfceae7128caef0fd160295d3c8` |
| Sheets / Inner - Landscape | 951 × 669 | `9acfd7b65736830d9416c502938ab6ab7b7a2620` |
| Sheets / Inner - Portrait - Folded | 669 × 951 | `a4181d237e41d2cef42acfa2d0ab90ee9e25e94d` |
| Sheets / Inner - Landscape - Folded | 951 × 669 | `634409ca9a2f1020898fd5e2f4e5ae59901f2800` |

Use the **Sheets** templates when the screen under test is a sheet or bottom sheet.

## Guide and part components (keys)

| Part | Key | Notes |
|---|---|---|
| Reserved Region (also used as "Fold Avoidance Region") | `56d56e4357db85b0f5105b9dfd4de9949d8c4242` | Variant of a set; resize to the region |
| Layout Margin: 20pt | `68f1b4fd4dd803e30a8fdccc0e47d480c1aa70d8` | |
| Safe Area Inset | `049e5ba3c6dd8e5476123e5a368499749ffb13ce` | |
| Outer Display Lens | `5e0583eccb0352930cc83ad080321f89e3f3b0ea` | ~96 × 96 camera marker |
| Toolbar - Top - iPhone Duo | `410deb22b13f17827d76155402dba8cabf46fad7` | Apple's own bar, for reference only |
| Tab Bar (inner portrait) | `135b8cf7b85ab1130dc1210e069c8bd414676e05` | Apple's own bar, for reference only |
| Component sets: `Tab Bar - iPhone Duo`, `Status bar - iPhone Duo`, `Toolbar - Top - iPhone Duo` | search by name | |

**Device frame** component set `iPhone Duo`: variants `Display=Inner|Outer, Color=Night Sky|Star White, Orientation=Portrait|Landscape`. The variant size equals the screen size; the bezel overflows it (outer portrait bezel 525 × 731 at −29,−27; inner bezel 749 × 1031 or 1031 × 749 at −40,−40).

## Geometry of each pose (from the templates)

| Pose | Screen | Side control rail | Camera reserved region | Margins | Fold avoidance region | Content area (approx.) |
|---|---|---|---|---|---|---|
| Outer portrait | 466 × 678 | right edge, 84 wide (safe area inset) | top right, 84 × 169 | 20 leading | none | ~362 wide (20 → 382), top bar 84 |
| Outer landscape | 678 × 466 | right edge, 84 wide | in the bottom right corner (lens at 582,370) | 20 leading | none | ~574 wide |
| Inner portrait | 669 × 951 | none: horizontal top bar 84 + bottom tab bar 95 | top right, 134 × 84 | 20 each side | none | 629 wide, 84 → 856 tall |
| Inner landscape | 951 × 669 | right edge, 84 wide | top of rail, 84 × 119 | 20 leading | none | ~847 wide |
| Inner portrait, folded | 669 × 951 | as inner portrait | as inner portrait | 20 each side | **horizontal band, y 456 → 496 (40 tall)** | two halves: top and bottom |
| Inner landscape, folded | 951 × 669 | right edge, 84 wide | 84 × 119 | 20 leading | **vertical band, x 456 → 496 (40 wide)** | two halves: left and right |

Note the fold runs **across** the screen in inner portrait (top and bottom halves) and **down** it in inner landscape (left and right halves).

## What it means for phone and tablet breakpoints

With a design system that switches to its tablet layout at 768pt (a common value; use the team's own if different):
- Outer portrait (466) and inner portrait (669) get the **phone** layout.
- Outer landscape (678) gets the **phone** layout.
- Inner landscape (951) gets the **tablet** layout.

So the same screen changes between phone and tablet layouts just by rotating the open device. Check both. Confirm with engineering that the app sees these point sizes as-is. On the webapp, Safari's own bars take some of the height.
