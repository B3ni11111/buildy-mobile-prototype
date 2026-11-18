> Purpose: the dated history of the redesign brainstorm (what was tried, what the user said). The current design is in design-spec.md, not here.

# Frontend redesign

Started 2026-11-09.

## Audit of the current look

- **Corners are everywhere:** `borderRadius: 4` (56px, 41 uses), `3` (42px, 18 uses), plus 2, 8, 10, 12, 14, 16, 22, 999 px. MUI multiplies numeric radius by the 14px base, so "4" turns into a pill-shaped card.
- **Two designs in one app:** onboarding (`onboarding/`, `SplashScreen`, `GradientWave`, `CrowdCanvas`) has its own gradients and drawn animations. `styles/Form.css` and `styles/InfoDisplay.css` add more green gradients. The dashboard and dialogs use plain MUI outlined cards.
- **Font drift:** code uses Rubik (`theme.ts` `FONT`, and 13 hardcoded `fontFamily` overrides), while `docs/designRules.md` and `.claude/guide/design.md` still say Georgia serif + system sans. The docs are stale.
- Per-component `sx` styling instead of theme tokens, so each screen drifts.

## Direction (user's choice)

- References: **Notion** (warm neutrals, flat, quiet, content first) + **Apple iOS Settings** (inset grouped lists, rows with chevrons, familiar on phones).
- Palette, fonts and corners are all open; the references decide. Forest green is not locked.
- Goal: simple and defined, not exciting, not sloppy. One design across onboarding and the app.

## Flow problems to fix

- **Add residents, "Set for everyone"** (`components/ResidentsEditor.tsx`, the `bulk` state + `applyToAll`): sets relation / floor / payment method for all pasted residents at once. Not practical; each apartment differs. To redesign.
- Onboarding and create-building wizard to be reviewed once the new style exists.

## Preview canvas (2026-11-09)

Mockups on a claude.ai design canvas: https://claude.ai/artifact/9ZN1uAMPcQgK1tuVWogcCA (private). Style tile, Home (light + dark), Members, Add residents (per-person rows, no "Set for everyone"), Onboarding name step.

Proposed tokens, not approved yet: Inter (Heebo for Hebrew); page `#F7F6F3`, surface `#FFFFFF`, text `#1F1E1C`, muted `#6B6862`, line `#E7E5E0`; default accent ink blue `#2F5D8A` (green, clay, ink as alternatives); corners 8px controls, 12px groups, round avatars; 1px borders, no shadows, no gradients; type 28/20/15/13.

## User's references (2026-11-10), mobile first, starting from Home

| Reference | What the user likes | Where in Buildy |
|---|---|---|
| Cash App numpad | big bold amount, full-screen keypad, pill buttons, one black Pay button | entering amounts: resident fee, calculator |
| Apple Wallet "Balance Details" | basic Apple layout: grouped rows, label left, value right in grey | user settings (and detail screens) |
| Wise landing | big bold headline, one friendly illustration, one button | Buildy intro / info page (the one with the people) |
| Revolut home | one big balance on top, round action buttons, list cards | money page: building balance, collected / goal |
| Fitbit (Google Health) | progress ring with big number "of 10,000" | money progress (collected / goal) |
| BitePal, Alan | illustrated hero with a character on Home | idea: drawn building with animated clouds and tiny people (variety, dogs), tab icons around it. User unsure, wants to discuss |

Observations to discuss: most refs use big confident numbers, a heavy sans, one bold brand color, and large but *consistent* corners (the old problem was inconsistency, not size). Building hero must respect privacy: never show which apartments paid.

## The vision, in the user's words (2026-11-10)

- "A new gen of design": **minimal but functional**. Results like BitePal's layout and Cash App's numpad.
- **Colorful, but only 3 colors**, each in darker and lighter shades.
- Home hero: a **simple minimal building** (e.g. 3 floors; floor count could come from building creation) on a **street scene that slowly rotates**. It looks 3D but is flat 2D, so only small shadows. A little playful, but playfulness is not the goal.
- Rejected: window = apartment motif, lit-window idea, a separate "signature element" hunt. The building scene is the element.

Palette chosen by the user (2026-11-10), 3 shades each, light to dark:
- Green (Buildy's): `#5C8064` · `#3A5A40` · `#2A4030`
- White: `#FFFFFF` · `#F1F0EC` · `#DEDCD5`
- Dark gray (not black): `#4B4E4B` · `#343634` · `#232523`

Scene contents: street ground, white cubes around the building, one streetlight, very tiny people like simple pictogram figures (one walking a minimal pet), walking in and out of the scene. Header: greeting + settings button, building name. Test board: `HomeScene.dc.html` on the canvas.

Feedback on scene test 1 (2026-11-10): turning speed is right. Make building AND ground white; ground becomes a thin plate only as big as the old sidewalk, around the building (room for people + street decoration); add one tree; green background. Look target: soft, cartoonish, minimal clay-like 3D (rounded edges, soft shadows). Building shape: bulky L, leaving open space on one side. User asked about a 3D library: proposed three.js (shapes are simple and generated from the floor count, so code beats a modelling tool).

Scene test 2 (2026-11-10): three.js page https://claude.ai/artifact/MUBQoym5WBa22RNsy7zPi3 (private). Flat 2D look: orthographic camera, a custom shader that paints every surface in exactly one of the 3 palette shades by how it faces a fixed light (no gradients), tiny flat contact shadows. Whole screen light green `#5C8064`, white text. L building, thin white plate, one tree, 3 white cubes, streetlight, 5 pictogram walkers (one with a pet, one walks out of the door).

Feedback on scene test 2 (2026-11-10): user liked it. Reference: a Tel Aviv Bauhaus corner building. Wanted: rounder building, small balconies, a real ground floor (entrance, shops, street level), all white with green light from some windows (not green or dark windows), no window grid, one bush, streetlight at the front, no random cubes, square plate. Scene test 3 (same URL) does this: L corner building with a big rounded corner, curved corner balconies plus small stacked balconies, recessed ground floor with columns, canopy and a lit entrance, irregular window ribbons that don't line up between floors, windows lighting up green now and then.

Feedback on scene test 3 (2026-11-11): user likes it. Changes in test 4: shading is fixed per side (the light turns with the scene, so no shade popping while rotating), windows always white with only a green glow around lit ones (no green panes), lights no longer switch on and off, more detailed tree (square tree pit, tapered trunk, branches, cluster crown) and bush (more blobs, tiny white flowers).

Scene test 5 (2026-11-11): potted plants on every balcony (3 plant shapes), streetlight moved to the front sidewalk with its lamp reaching in toward the entrance (it used to hang out over the edge), a bench in the back yard facing the tree, and lit windows now get an even green tint over the whole glass plus a soft halo with no bright centre.

2026-11-11 tweaks: plants on only ~40% of balcony spots; streetlight at the front left by the bush (lamp reaching in); bench against the back wall facing out; rounded plate corners. Then a "cartoon try" (version 8): softly rounded corners on the whole L, puffy rounded edges on the ground, slabs, balconies and roof, a thicker ground plate, dark-green outlines on all plants, two puffy outlined clouds drifting overhead, bouncier walkers. Pre-cartoon copy kept in the scratchpad as `buildy-scene-before-cartoon.html` (also artifact version 7).

User rejected the cartoon try: back to version 7, no outlines, plus two puffy white clouds that turn with the scene and bob gently (version 9). The cartoon copy stays in the scratchpad as `buildy-scene-cartoon.html`.

Later tweaks (versions 10 to 13): clouds about 50% bigger, drifting slowly back and forth while still turning with the scene; light now comes from the L's inner corner side, so shade and ground shadows fall diagonally toward the rounded outer corner; plants use only the two lighter greens (the darkest green made a rim that read as an outline).

## Home screen (2026-11-11)

Decided with the user: the full progress ring lives on the Money tab; Home shows a small version in a card ("num / num", small gray month title top left). Residents see their own data first: their payment, issues, door code. BitePal-style floating tab bar: yes. White cards sit on the green: yes. Palette added to `docs/designRules.md` as provisional, for dev consistency.

Built on the scene page: a wide money card (month, ₪6,300 / ₪9,600, "16 of 24 apartments paid", small ring), two half cards (Issues: 3 open; Door code: tap to show), floating tab bar Home / Money / Issues / People, test switch Committee / Resident (resident sees ₪400 / ₪400, paid).

## Money tab (2026-11-11)

Decided with the user: one ring, same as the Home card but large; swipe between months; same green background with white cards; the special Cash App-style numpad is only for the calculation process (fees typed elsewhere will use the phone's own keyboard later); the numpad opens as a sheet sliding up from the bottom.

Built on the same prototype page (tab bar now switches Home / Money):
- Swipeable month cards (Sep, Oct, Nov), each with the big ring (collected of goal, apartments paid), dots to jump between months.
- Head: round actions (Mark all paid, Calculate, Payment log, Settings; the last two not built), apartments list with All / Unpaid / Paid filters; tapping a current-month row marks it paid or undoes it; past months are read-only.
- Resident: own ring, demo Pay button while unpaid, own payment history; the Home card follows the same data.
- Calculate: green sheet with the monthly budget, keypad, "Split equally" / "By apartment size", Apply to the current month only (past months keep their amounts).

## Screens still to design (added 2026-11-13)

- **Success pages** for the main flows, since the app is a demo and people need clear "it worked" feedback: payment done (demo, no real money), signed in, building created, and the same for other finished flows (resident added, issue reported, document uploaded). One shared success layout in the new style, reused by every flow.

Idea (user, 2026-11-11): tapping the building opens an action ring of quick actions. Theirs: report an issue, upload a document; no Pay in the ring. Under discussion: announcement, invite a neighbour / add resident, contact the committee, start a vote, and whether the ring differs per role.

Implementation thought: a flat-shaded diorama (CSS 3D or SVG) where each face uses a shade of the 3 colors, so the palette doubles as the lighting.

## Issues tab (2026-11-13, in discussion)

Proposed and waiting for the user's answers; see design-spec.md, "Issues tab".

## Handoff (2026-11-17)

Everything was gathered into this folder (`mobile/`, moved there from `docs/temporary/redesign/` on 2026-11-18) so a new Claude Code session can rebuild the design from it. The old "next steps" (Notion + iOS tokens) were dropped: that direction was rejected on 2026-11-10.

## v2 in the new look (2026-11-17)

The user said the new design rules (from the Rive animations: bright green, white, flat side shade) override this spec. Built `prototype/buildy-mobile-v2.html`: the building scene code is unchanged except its greens (`0x4FE57A, 0x06D63E, 0x04A32E` and the halo color). New: big white titles with a solid green shade under the letters, dark small text on the green for contrast, Duolingo-style pressable buttons, a tab bar with a raised green + button (Report an issue, Upload a document), Issues tab with counts and an issue sheet with a tracking bar, People grouped by floor, Settings as grouped rows (floors stepper drives the building), and a waiting then success screen in the same language as the Rive animations (every third upload shows the error). Waiting for the user's feedback.

## Real Rive animation in v2 (2026-11-17)

Upload a document now plays the real Rive file (embedded) with three test buttons: Success, Too heavy, Failed. Success B (big cabinet) is now the main one, about 4 in 5; A is the rare one.

Weights changed the same day: success B 3, A 2 (B about 60%).

Door code in v2 (same day): after a copy the V shows for a full 3 seconds with a fresh timer ring, then the card resets. Copy falls back to an older method when the clipboard API is blocked.

## Upload form and phone link (2026-11-17)

Upload a document opens a form. On Upload the form card turns into the folder at once (gray back and tab appear behind it), then flies and shrinks onto the Rive folder at rest (measured from the canvas, within 1 px), so the hand-off is invisible. The server answer is random (about 72% success, 14% failed, 14% too heavy; files over 10 MB are always too heavy) and can be forced in Settings > Prototype > Upload result. A phone copy (Rive engine bundled inside) is published privately at https://claude.ai/artifact/FDZb5as2X5gggQuHi3ekWm.

## Open: green on phones (2026-11-17, parked by the user)

The user sees `#06D63E` differently on the iPhone (lighter, more lime) than on the computer, and prefers the computer look. Display P3 with the same numbers (`color(display-p3 0.024 0.839 0.243)`) did not match either. Tools left in the prototype under Settings > Prototype: Match the green (hue, strength, brightness sliders with the code shown), Green strength (overall softening), Phone colors (Standard or Vivid). When the user picks a code, change the app tokens and the greens inside the Rive file to it, re-export, and remove these test tools.
