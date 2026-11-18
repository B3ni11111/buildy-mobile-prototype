> Purpose: the entry point to Buildy's redesign handoff: what's in this folder and how to rebuild the design from it.

# Buildy redesign: handoff folder

This folder holds everything from the redesign brainstorm (2026-11-09 to 2026-11-17), so a new Claude Code session can recreate the design in the real app without the original conversation.

## Read in this order

> Precedence (user, 2026-11-17): `docs/designRules.md` and `docs/designRules.mobile.md` override this folder where they disagree (bright green `#06D63E`, flat side shade, pressable buttons). The newest prototype is `prototype/buildy-mobile-v2.html`.

1. **`design-spec.md`:** the complete current design. Tokens, every screen, the 3D building scene with exact values, what's still open, and what was rejected. Start here.
2. **`prototype/buildy-prototype.html`:** the working prototype the user approved: Home (turning building, cards, door code), Money tab (swipeable month rings, apartments list, resident Pay) and the calculator sheet. When the spec and the prototype disagree, the prototype wins for visuals, the spec wins for decisions.
3. **`references/`:** the user's reference images. The spec says what each one is used for.
4. **`log.md`:** the dated history: what was tried, what the user said, why things changed. Read it when you need the reason behind a decision.

## Files

| File | What it is |
|---|---|
| `design-spec.md` | The full design spec |
| `log.md` | Dated history of the brainstorm |
| `prototype/buildy-mobile-v2.html` | v2 (2026-11-17): the whole app in the new playful minimalism look. Same building code with the new greens; Home, Money, Issues, People, Settings, + action sheet, waiting and success screens. Not approved yet |
| `prototype/buildy-prototype.html` | Approved prototype (HTML + three.js r128 from cdnjs) |
| `prototype/rejected-cartoon-try.html` | A rejected version of the scene, kept only to show what not to do |
| `references/01-cashapp-numpad.png` | Cash App: big amount, keypad, pill buttons, dark Pay → the calculator sheet |
| `references/02-apple-wallet-balance-details.png` | Apple Wallet: grouped rows, label left, value right → Settings, lists |
| `references/03-wise-landing.png` | Wise: huge headline, one illustration, one button → intro page |
| `references/04-revolut-home.png` | Revolut: big balance, round action buttons, list → Money tab |
| `references/05-bitepal-home.png` | BitePal: illustrated hero, white cards, floating tab bar → Home layout |
| `references/06-alan-home.png` | Alan: illustrated hero with cards → Home layout idea |
| `references/07-fitbit-progress-ring.png` | Fitbit: progress ring with a big number → money ring |
| `references/08-pictogram-people.png` | Pictogram figures → the tiny walking people |
| `references/09-cartoon-minimal-building.png` | Minimal white building on a plate → the scene's look |
| `references/10-l-shape-building.webp` | Bulky L-shaped building → the building's shape (stock watermark, shape reference only) |
| `references/11-tel-aviv-corner-building.webp` | Tel Aviv Bauhaus corner building → rounded corner, balconies, ground floor |

## Viewing the prototype

- Open `prototype/buildy-prototype.html` in a browser (it's a page fragment without `<html>`/`<head>`, which browsers handle fine). It needs internet for three.js and Google Fonts.
- Published copy, private to the user: https://claude.ai/artifact/MUBQoym5WBa22RNsy7zPi3
- Use a phone-width window (about 400 px). The "Test controls" panel switches between **Committee** and **Resident** views, changes the floor count, the turn speed, and pauses the scene.

## How to rebuild

- Build it into the real frontend (`frontend/`, Vite + React + MUI), replacing the old theme and per-component styles. Section 11 of the spec lists what to replace and which data already exists.
- Keep every color behind a token: the shades may still change. The palette is also in `docs/designRules.md`.
- Keep the building scene generated in code from the floor count (three.js or React Three Fiber), using the flat 3-shade shader approach from the prototype.
- Ask the user the open Issues questions (spec section 8) before designing that tab, and keep designing together: open questions and the user's own references, one decision at a time, never "guess and ask for approval".
- Repo rules: test with `docker compose up`, never push to production, and don't commit or push unless the user asks.
