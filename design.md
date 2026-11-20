# eQc Draw — Design Rules

The rules the UI must follow. If the prototype and this file disagree, one of them is wrong. Fix it the same day, and don't leave the drift.

**Source of truth:** the Figma file *EQC-Prototype v1* (`IJgQKF1et5qnYRZO3fhvXG`), page "ECQ prototype". Order of authority: Figma > this file > `index.html`. If `index.html` does something Figma doesn't show, it's either listed under [Known deviations](#known-deviations) or it's a bug.

---

## 1. Principles

1. **Hand-drawn, not slick.** Heavy ink outlines, offset shadows, handwriting accents. It should feel like a notebook.
2. **One primary action per screen**, always the lime button, always bottom-centre.
3. **iPad first.** The canvas is 834×1194 (iPad Pro 11" portrait). Touch targets are ≥ 50px tall.
4. **The drawing is the product.** Nothing competes with the canvas on the draw screen.
5. **Hidden until the reveal.** Players only ever see the bottom edge of the previous drawing.

## 2. Tokens

Defined as Figma variables in the collection **eQ tokens** and as CSS custom properties in `:root`. Don't hard-code hex values in components. Add a token first.

| Token | Value | Use |
|---|---|---|
| `--paper` | `#fffdf8` | Screen background |
| `--card` | `#fffdf7` | Cards, chips, unselected buttons |
| `--ink` | `#1a1a1a` | All text, outlines, shadows |
| `--mute` | `#8a8673` | Secondary text, dividers between tool groups |
| `--accent` | `#ff5a36` | Timer bar, DONE / PLAY AGAIN, overlap guide, `#1`/`#2` ranks. **Changes per theme** (Monster orange, Robot blue, Alien green, Pirate red, Dragon purple) |
| `--go` | `#84cc16` | Primary action buttons only. Never changes with theme |
| `--line` | `#e5e5e5` | Hairlines on the login card only |
| white | `#ffffff` | Canvas, login card, avatar fill |

Rules:
- Text is `--ink` or `--mute`. No other text colours except the low-time timer red `#d6232a`.
- `--go` is reserved for the action that moves the game forward. Don't use it for decoration.
- Selected friend row fill: `#fff1ed`.

## 3. Typography

Three families, three jobs. Don't mix the jobs.

| Family | Weights | Job |
|---|---|---|
| **Faustina** (serif) | 600, 700 | Logo wordmark, primary buttons, theme titles |
| **Inter** (sans) | 400–700 | All UI text: body, labels, chips, tools, tagline |
| **Kalam** (handwriting) | 400, 700 | Timer, "ta-da!", theme prompts, input placeholder, guide label |

Common sizes (px): primary button 36 · wordmark 44 (130 on onboarding) · theme title 60 · "ta-da!" 64 · screen heading 48 · notification 36 · body/prompt 20 · tool/chip/pill 16 · login card text 14.

## 4. Shape, borders, shadows

- **Outline:** `--ink`, 3px on chips/tools/pills, 4px on buttons, cards, inputs and the canvas, 4px on the theme card.
- **Offset shadow:** `6px 6px 0 --ink`, no blur. Used on primary buttons, pills, the canvas, the reveal frame and selected friend rows. The small ADD pill uses 4px. The pressed state shifts the element 4px and shrinks the shadow to 2px.
- **Radii:** pill buttons 60 · cards 22–30 · tool buttons 18 · size buttons 16 · inputs 16 (login inputs 8) · canvas 8.
- The login card is the one exception to the heavy look: white, 1px `--line`, radius 14, subtle shadow.

## 5. Components

| Component | Rule |
|---|---|
| **Logo (wide)** | Snail image, then "eQc" (Faustina 700) with "Draw" (Inter 700, `#262626`) stacked under it. Top-left of every in-game screen, centred on reveal. |
| **Logo (stacked)** | Onboarding only: snail above, wordmark below, centred. |
| **Primary button** | Lime, Faustina 600 36px, arrow icon after the label, 4px outline, 6px shadow. Label is the next step: `Get Drawing`, `Draw`, `SEND`, `HEAD/BODY/LEGS START`, `Login`. |
| **Pill button** | Inter 500, `--card` fill, 3px outline, 6px shadow. `DONE` and `PLAY AGAIN` use `--accent` fill. |
| **Chip** | Inter 500 16px, 3px outline, radius 30. Shows where you are (`choose a theme`, `HEAD 1/3`, `send to`, `pass the iPad`). Not tappable. |
| **Tool button** | Icon + label, 3px outline, radius 18. Selected state is an `--ink` fill with white text and icon. Exactly one drawing tool is selected at a time. |
| **Size button** | 60×60, three sizes (6 / 12 / 22px dot). Selected is inverted. |
| **Colour swatch** | 60px circle, 4px outline, 2 columns × 4 rows. Selected has a 5px `--accent` ring, offset 3px. |
| **Friend row** | 108px tall, avatar (initial in a 66px ring), name, rank (`#1` / `#2`) on the right. Max two selected. |
| **Overlap guide** | Dashed `--accent` line 56px above the canvas bottom, with a Kalam label "let lines run past here ↓". |
| **Icons** | Single 24px set, drawn in `currentColor` so they follow the text colour. Source SVGs live in `assets/`. |

## 6. Layout

- Stage 834×1194, scaled to fit the viewport, corner radius 28. Content gutters are 40px.
- Top bar (logo + chip + actions) sits 36–56px from the top. Primary button sits 48px from the bottom.
- The draw screen is a fixed stack: top bar → timer bar (12px) → brief row → canvas (760×500) → tools row. Don't add anything between the top bar and the canvas.
- The canvas is 760×500 with a 56px overlap strip. Strip height is a game rule, so don't change it casually.

## 7. Screen flow

`0 Login → 1 Onboarding → 2 Theme → 3 Draw (head) → 4 Send → 5 Pass the iPad → 6 Draw (body) → 5 Pass → 6 Draw (legs) → 7 Reveal → 2 Theme`

- Send is shown once, after the head. The chain is then fixed.
- Pass screens use the real bottom edge of the previous drawing, not a placeholder.
- Reveal unfolds head → body → legs, then offers SAVE PNG and PLAY AGAIN.

## 8. Behaviour rules

- **Timer:** 90 seconds per part. Timer text and bar turn `#d6232a` at ≤ 10s. At 0 the turn ends automatically.
- **Apple Pencil:** pen strokes are pressure-sensitive. After a pen touch, finger touches are ignored for 4s (palm rejection).
- **Undo/redo:** Undo button and Cmd/Ctrl+Z. Redo is Cmd/Ctrl+Shift+Z only, with no button.
- **Eraser** switches back to Pen when a colour is picked.
- **Theme** sets the accent colour and the 8-colour palette. The palette order puts black first.
- **Next player** sees only the strip, and the note says so. Don't reveal more.

## 9. Keep-us-honest rules

Before merging any UI change:

1. **Figma first.** Visual changes start in Figma, then land in code. If you changed code first, update Figma in the same PR or log it below.
2. **No new colours, fonts, sizes or radii** without adding them to this file. If you reach for `#something`, stop and add a token.
3. **Screenshot diff.** Compare each touched screen against its Figma frame at 834×1194. Differences are either fixed or listed under Known deviations.
4. **One primary button per screen**, lime, with an arrow. Anything else is a pill.
5. **Copy comes from Figma.** Don't rewrite labels in code. Fix the Figma text instead.
6. **Check the full loop.** Run login → reveal once, including the pen tools, before saying "done". Check that the console is clean.
7. **Placeholder means placeholder.** Anything not designed yet is listed below. It doesn't quietly become the design.
8. **Update this file** when a rule changes. Rule changes ship with the code that needs them.

## 10. Known deviations

Things in the prototype that don't (yet) match Figma. Clear each one by fixing the code or the design, then delete the line.

| Area | Gap | Owner |
|---|---|---|
| Themes | Only **Monster** is designed. Robot, Alien, Pirate and Dragon use placeholder art made in code. | Design |
| Draw | The "legs" turn has no Figma frame. It reuses the body layout. | Design |
| Copy | Tagline "A collaborative drawing game for creating together and" is cut off in Figma (Login, Onboarding). Copied as-is. | Design |
| Copy | Colour column is headed "Correction Tools" in Figma, which looks like a copy error. Code says "Colors". | Design |
| Tools | **Marker** has no spec beyond its icon. Code uses 80% opacity, medium width. | Design |
| Pass screen | Figma's strip is an empty box with two dividers. Code shows the real bottom edge of the previous drawing. This is intentional. | Both |
| Login | Prototype only: any login button continues. No validation, no real auth. | Eng |
| Send | The friend input accepts an email but only keeps the part before the "@". No invitations are sent. | Eng |
| Fonts | Faustina, Inter and Kalam load from Google Fonts. Offline, it falls back to Georgia / system sans / cursive. | Eng |
