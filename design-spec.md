> Purpose: the complete, current spec of Buildy's new mobile design, detailed enough to rebuild it in the real app.

# Buildy redesign: design spec

Status on 2026-11-17: **Home and Money are designed and agreed** (working prototype in `prototype/buildy-prototype.html`). **Issues** is proposed and waiting for the user's answers. Settings, intro page, People, success pages and the building action ring are not designed yet.

Mobile first: everything here is designed for a phone (390 to 420 px wide). The prototype is the source of truth for anything this file doesn't spell out.

---

## 1. The idea behind the look

In the user's words: **"a new gen of design, minimal but functional"**. Not exciting and not playful for its own sake, but never sloppy. Target results like the apps in `references/` (BitePal's layout, Cash App's numpad, Revolut's money screen, Apple's lists).

- **One signature element:** a small white Tel Aviv-style building that slowly turns on the Home screen, on a little street with tiny walking people. It looks 3D but is drawn flat.
- **Only 3 colors**, each in 3 shades. The shades do the layering (cards, pressed states) and the "lighting" of the 3D scene.
- **Big, confident numbers** for money and counts.
- **Consistent shapes:** the same few corner sizes everywhere. The old app's problem was 15 different radiuses, not big ones.
- **Trust and privacy** (from CLAUDE.md): residents never see other residents' payment state. This applies to every screen and to the building scene.

## 2. Tokens

### Colors (also in `docs/designRules.md`; shades not final, so always use tokens)

| Token | Value | Use |
|---|---|---|
| `green-1` | `#5C8064` | App background (whole screen), lit-window glow, light leaves |
| `green-2` | `#3A5A40` | Progress ring arc, paid check circles, calculator sheet background, inactive month dots |
| `green-3` | `#2A4030` | The flat "lip" shadow under white elements, plate shadow |
| `white-1` | `#FFFFFF` | Cards, tab bar, buttons on green, roofs and ground in the scene |
| `white-2` | `#F1F0EC` | Pressed/active fills inside cards, icon circles, row dividers, lit walls in the scene |
| `white-3` | `#DEDCD5` | Ring track, shaded walls in the scene |
| `gray-1` | `#4B4E4B` | Secondary text (labels, subtitles) |
| `gray-2` | `#343634` | Icons |
| `gray-3` | `#232523` | Main text, dark buttons (Pay, Apply), pictogram people. Never pure black |

Rules: no other hues, no gradients. Dark gray is for text and small details, never large building surfaces. The app is a single green look (no dark mode designed yet).

### Type

- Font: **Inter**, with **Heebo** as fallback for Hebrew (`'Inter', 'Heebo', system-ui, sans-serif`). Both from Google Fonts.
- Page title: 32/38, weight 700, letter-spacing -0.02em (building name on Home, "Money").
- Big card numbers: 30/36, 700, tabular numbers. Ring center number: 34/40, 700.
- Body and list rows: 15, weight 500 for titles.
- Labels and subtitles: 13, weight 500, `gray-1`.
- Tab labels: 12, 500 (600 when active).
- Calculator amount: 56/64, 700, letter-spacing -0.03em.

### Shape and depth

| Element | Corner |
|---|---|
| Home cards | 24 px |
| List groups (Money list) | 20 px |
| Calculator sheet (top corners) | 28 px |
| Buttons, pills, tab bar, segmented controls | fully round (999 px) |
| Icon buttons, avatars, action buttons | circles |
| Segmented control container / its buttons | 12 px / 9 px |

- **Shadow:** white elements on the green get a flat lip, `0 3px 0 green-3`, no blur. No soft drop shadows anywhere.
- Spacing: 16 px side gutters, 12 px between cards, 8 px base unit. Touch targets at least 44 px.

## 3. App shell

- Phone column, max width 420 px, centered, `green-1` background edge to edge.
- **Floating tab bar** at the bottom (BitePal style): a white pill 16 px above the bottom safe area, width `min(388px, 100% - 32px)`, lip shadow, 4 equal tabs: **Home, Money, Issues, People**. Each tab is an icon with a small label under it (labels kept for older residents). The active tab gets a round `white-2` fill. Page content has bottom padding so the bar never covers it.
- **Header** on each tab: a small 15 px white line (greeting on Home, building name elsewhere), then the big white page title.
- **Toast** for quick feedback: a dark `gray-3` pill above the tab bar, white 14 px text, fades in and out (about 2 seconds).
- **Bottom sheets** slide up from the bottom over a dimmed backdrop (`rgba(35,37,35,0.45)`): 0.38 s, `cubic-bezier(0.2, 0.8, 0.2, 1)`, backdrop fades in 0.3 s. Tap outside or Escape closes. With reduced motion: no animation.

## 4. Home tab

Order from top:

1. **Header:** "Good evening, Dana" (white, 15/500) on the left; a 44 px white round **Settings** button (gear icon, `gray-2`) on the right. Settings will use the Apple Wallet style (`references/02`), not designed yet.
2. **Building name** as the page title: "Herzl 12".
3. **The building scene** (section 5), full column width, aspect ratio 1 : 1.1.
4. **Cards** (white, 24 px corners, lip shadow), in a 2-column grid:
   - **Money card, full width.** Left: the month ("November", 13/500 `gray-1`) at the top left, the amount as **"₪6,300 / ₪9,600"** (first number 30/700 `gray-3`, "/ goal" 17/500 `gray-1`), then "16 of 24 apartments paid". Right: a small progress ring, 76 px, radius 31, stroke 9, `white-3` track, `green-2` arc with round caps, percentage in the middle (15/700). Tapping the card opens the Money tab.
   - **Issues card, half width.** Label "Issues", a 36 px round `white-2` icon at the top right, big "3", "open in the building" (resident: "open, 1 reported by you").
   - **Door code card, half width.** Label "Door code", key icon in the round icon spot, big "••••", "Tap to show". Behaviour:
     - Tap: shows the code for **3 seconds**. The key icon turns into a **copy icon** inside a thin `green-2` **timer ring** (36 px circle, stroke 2.5) that drains over the 3 seconds. Subtitle "Tap again to copy".
     - Tap again within the 3 seconds: copies the code. The copy icon turns into a **check (V) icon**, subtitle "Code copied". The ring **keeps draining to the end**, then everything resets to the key icon and "••••". Further taps during that time do nothing.
     - If the clipboard is blocked: "Copy not allowed here".

What each role sees (privacy):
- **Committee head:** building totals in the Money card.
- **Resident:** only their own fee, for example "₪400 / ₪400", "Your fee is paid" (or "₪0 / ₪400", "Not paid yet"). Never the building total or anyone else.

## 5. The building scene (Home hero)

The signature element. In the prototype it's three.js r128 (`prototype/buildy-prototype.html`, the big script at the bottom). The real app can use three.js or React Three Fiber; keep it generated in code from the floor count, not a hand-made model.

### Look

- **Flat 2D that turns in 3D.** Orthographic camera (no perspective) looking from 30° above. Only the scene turns; the camera stays still.
- **Turning speed:** one full turn every **60 s** (30 / 60 / 90 s were tested; 60 s was "great"). Starts at a 0.55 rad angle.
- **Flat 3-shade shading, no gradients.** A custom shader paints every surface in exactly one palette shade by how it faces the light: `dot(normal, light) > 0.62` → shade 1, `> 0.04` → shade 2, else shade 3.
- **The light turns with the scene**, so each side keeps a **fixed** shade while it rotates (no shade popping).
- **Light direction:** from the L's inner corner side, vector `(-0.6, 1, -0.6)` normalized. Shade and ground shadows fall diagonally toward the rounded outer corner. Walls facing the yard are light white; the front, the right side and the rounded corner are the darker white.
- **Window glass** is always one shade lighter than the wall it sits in (reads on both lit and shaded walls). Glass stays white.
- **Leaves** use only the two lighter greens (`dot > 0.5` → green-1, else green-2). The darkest green made a rim that read as an outline, which the user didn't want.
- **No outlines anywhere** (building, plants, clouds). **No black or dark surfaces** on the building.
- **Shadows:** small flat contact shadows (`gray-3` at 12% opacity) under the building, plants, streetlight and bench, offset diagonally toward the outer corner. The plate has a flat `green-3` 45% shadow offset (3, 3).
- Reduced motion: the scene starts paused at its starting angle.

### Contents (units are scene units; the plate is 88 wide)

- **Ground:** a thin, square white plate, 88 × 88, 2 thick, **rounded corners (radius 8)**. Just big enough around the building for people to walk and for the street pieces.
- **Building:** a bulky **L-shaped Tel Aviv / Bauhaus corner building** (`references/11` for style, `references/10` for the L shape), all white.
  - Footprint: a front wing along the street (x −25 to 25, z 6 to 30) and a right wing going back (x 1 to 25, z −30 to 30). The open corner of the L is a back yard.
  - **Big rounded corner** where the two wings meet at the front right: radius 14, center (11, 16). Other corners are sharp in the agreed version (rounding them all was part of the rejected "cartoon try").
  - **Ground floor**, 11 high: set back from the floors above (offset −3), with thin round columns in front (radius 0.55, every 9 units along the outline) and a **canopy** all around (offset +0.7, 1 thick).
    - **Entrance** at the front (x −12), with a lit doorway, its own small canopy (7 × 0.6 × 4.2) and a step.
    - **Shop windows:** tall (7 high).
  - **Upper floors:** 10 high each. The count comes from the building's floor number (default 3; the head would pick it when creating the building). Walls offset −1.2; a thin slab band (offset +0.4, 0.8 thick) between floors gives the horizontal Tel Aviv lines.
  - **Curved corner balconies** on every floor around the rounded corner: a slab from radius 12.4 to 17.4 (0.9 thick) with a low solid railing (radius 17 to 17.4, 3 high).
  - **Small straight balconies,** stacked: one on the right side (z −14) and one on the front (x −14) on every floor.
  - **Roof:** a slab (offset +0.5, 1.2 thick), a slightly darker inset (`white-2`, offset −2.2) that reads as a rim, and a small stair room (9 × 4 × 7).
  - **Windows: no grid.** Each floor gets its own irregular "ribbons" of windows along the outline, from a random generator seeded by the floor count, so floors never line up. On the curve the windows follow the bend.
    - Ground floor runs: 7 to 13 units long, gaps of 2 to 4, 7 high.
    - Upper floor runs: 4 to 10 long, gaps of 3 to 7, 4.8 high.
  - **Lit windows:** about 45% of shop windows and 20% of upper windows. A lit window keeps white glass with an **even green tint over the whole glass** (`green-1`, 32% opacity). On straight walls there's also a soft halo around it that is even in the middle and fades only at the edges (**no bright dot**). Lights stay fixed; they don't switch on and off.
  - **Potted plants** on about **40% of balcony spots** (not all): small white pots with one of 3 plant shapes (round, tall stacked, wide low).
- **Tree** in the back yard at (−12, −16): a small white square tree pit, a tapered gray trunk with 2 small branches, and a crown of 6 overlapping round leaf clusters.
- **Bush** at the front left (−38, 24): 6 leafy clusters with 11 tiny white flowers.
- **Streetlight** at the front left by the bush (−39, 33.5): a gray pole, the arm reaching **inward** toward the building (never out over the edge), a white lamp.
- **Bench** against the back wall of the building at (13, −33.2), facing out to the back sidewalk: white seat and backrest, small gray legs.
- **Clouds:** 2 puffy white clouds (4 overlapping balls each; scales 1.85 and 1.4) above the building. They are **part of the turning scene** and also **drift slowly** back and forth (about ±14 units across, ±6 in depth, roughly a minute per drift) and bob gently.
- **People:** 6 tiny pictogram figures drawn like `references/08` (round head, rounded shoulders, two legs, `gray-3`), always facing the viewer. One walks a simple pet on a leash, one is child-sized, one walks out of the entrance. They walk along the edges of the plate, bob a little, and fade in and out at the ends of their paths.
- **No random cubes** on the street (an early test had them; the user rejected them).

### Camera framing

The view zooms so the plate and the top of the building (plus clouds) always fit, whatever the floor count.

## 6. Money tab

The full version of the Home money card. Same green background and white cards. References: Revolut (`04`) for the structure, Fitbit (`07`) for the ring, Apple Wallet (`02`) for the list.

1. **Header:** "Herzl 12" (15/500 white), title "Money".
2. **Month cards you swipe left and right** (scroll snap, one month per page). Each is a white card with:
   - "November 2026 · this month" (13/500 `gray-1`),
   - a **big ring**: 200 px, radius 84, stroke 16, `white-3` track, `green-2` arc with round caps,
   - in the middle **"₪6,300"** (34/700) and "of ₪9,600" (15/500 `gray-1`),
   - "16 of 24 apartments paid" under it.
   Small dots under the cards show the month and can be tapped to jump there (for people who don't swipe). Active dot white, others `green-2`.
3. **Committee head only:** 4 round action buttons (Revolut style): a 54 px white circle with a `gray-2` icon and lip shadow, white 12 px label under it.
   - **Mark all paid**
   - **Calculate** (opens the calculator sheet, section 7)
   - **Payment log** (not designed yet)
   - **Settings** (not designed yet)
4. **Resident only:** a full-width dark **Pay** button (56 px, `gray-3`, white 17/600 text), "Pay ₪400 · demo", shown only while their month is unpaid and only to whoever pays for the apartment.
5. **List** (Apple Wallet style), title in white 17/600 with filters on the right.
   - **Committee head:** "Apartments · November" with **All / Unpaid / Paid** filters (a translucent white segmented control on the green). A white group of rows: "Apt 9 · Noa Peretz", the amount, and either a `green-2` circle with a white check or "Unpaid" in `gray-1`. In the current month, tapping a row marks it paid (tap again to undo). Past months are view only. The list follows the month you swiped to.
   - **Resident:** "Your payments · Apartment 9", one row per month with amount and status. No filters, no other apartments.

Everything updates together: the ring, the list and the Home money card.

## 7. Calculator sheet (the special numpad)

Cash App style (`references/01`). The user's rule: **this numpad is only for the calculation process.** Other fee fields will use the phone's own number keyboard later.

- Opens from **Calculate** as a bottom sheet sliding up (section 3), background `green-2`, white text, 28 px top corners, a small grabber bar.
- Contents, top to bottom:
  - "Monthly budget" label (`white-3`),
  - the amount, "₪12,400" (56/700),
  - a live result line (`white-2`): "₪400 per apartment" or "₪310 to ₪520 per apartment, by size",
  - a **3 × 4 keypad** (1–9, ".", 0, delete), transparent keys with white 26/600 digits and a faint press fill. Up to 7 digits and 2 decimals,
  - two pill toggles (Cash App's "Pool / Request"): **Split equally** / **By apartment size**. Selected: white with `gray-3` text; other: translucent white,
  - a dark full-width **Apply to November** button (disabled until there is an amount).
- Apply sets the **current month's** fees only; past months keep their amounts (matches `docs/temporary/payments.md`). Then a toast: "November fees updated. Past months keep theirs".

## 8. Issues tab (proposed, waiting for the user)

How issues work today (see `docs/dataStructure.md`): visible to the whole building; status **Opened → In progress or Denied → Resolved** (closed issues never change); updates with text and photos while in progress; full history; the author can edit or delete while it's open and "ping" once a week to bring it to the top.

Proposal shown to the user:
- Title "Issues", filters **Open · In progress · Closed** with counts (same pill style as the Money filters).
- A big dark **Report an issue** button.
- White cards per issue: status dot and word, bold title, one line of description, "Dana · 3 days ago", a photo thumbnail on the right when there is one.
- Opening an issue: a bottom sheet with photos, a **delivery-style tracking bar** (Reported → In progress → Resolved), the update history as a timeline, and role-based buttons (head: Accept, Deny, Add update, Mark resolved; author: "Still a problem" = the ping, Edit, Delete while open).
- Reporting: the same sheet with title, description, photos, Send, then a success page.

**Open questions for the user** (ask before building):
1. Delivery-style tracking bar, or a simple colored status word?
2. Should a report say **where** in the building (lobby, elevator, stairs, roof, parking, garden, my apartment)? That isn't in the data today. Later these could show as pins on the building model.
3. Cards with photo thumbnails, or compact Apple-style rows like Money?
4. Should everyone see who reported an issue, or only management (residents see "a neighbour")?
5. Keep the weekly ping, and is "Still a problem" the right wording?

## 9. Still to design

- **Success pages** for finished flows (the app is a demo, people need clear "it worked" feedback): payment done (demo, no real money), signed in, building created, resident added, issue reported, document uploaded. One shared success layout, reused by every flow.
- **Settings** in the Apple Wallet style (`references/02`): grouped rows, label left, value right in gray.
- **Intro / info page** (the one with the people) in the Wise style (`references/03`): one huge headline, one friendly illustration, one button. Take the feeling, don't copy Wise.
- **People tab**, including adding residents **without** "Set for everyone": each pasted person gets their own row for apartment, floor and owner/renter (the old bulk "Set for everyone" in `frontend/src/components/ResidentsEditor.tsx` is not practical).
- **Onboarding** and **Create building** restyled into this look (create building would also ask for the number of floors, which drives the scene).
- Future ideas, already in `docs/ideas.md`: the **action ring** when tapping the building (report an issue, upload a document, never Pay), and a **bell button** on Money to remind unpaid residents (manual, never automatic).

## 10. Rejected directions (don't bring these back)

- **Round 1 "quiet style"** (Notion + iOS Settings: muted neutrals, ink-blue accent, 8/12 px corners). Rejected as guesswork; the user's own references replaced it. Canvas: https://claude.ai/artifact/9ZN1uAMPcQgK1tuVWogcCA (private).
- "One window = one apartment" / lit-window-per-apartment motif, and a separate "signature symbol" hunt. The building scene is the element.
- Dark or black windows, green window glass, a regular window grid, windows switching lights on and off.
- Random white cubes on the street; a big green grass slab with a dark street around the building.
- Shade popping as the scene turns (fixed by turning the light with the scene).
- A green light glow with a bright dot in the middle.
- Potted plants on every balcony.
- The **"cartoon try"**: rounded corners on the whole L, puffy beveled edges, a thick toy-like base, dark outlines on plants and clouds, bouncier walkers. Kept for reference only in `prototype/rejected-cartoon-try.html`.
- Clouds floating on their own, not turning with the scene.

## 11. Notes for building it in the app

- Stack: Vite + React + MUI (`frontend/`). The old look lives in `frontend/src/theme.ts` (forest green on cream, navy dark mode, Rubik, 14 px base radius that multiplies numeric `borderRadius`) plus many per-component `sx` overrides and gradient CSS (`styles/Form.css`, `styles/InfoDisplay.css`, onboarding). Replace with the tokens above and remove the overrides.
- Data already exists for Home and Money: `payments` (per apartment per month, `unpaid`/`paid`, `amount` snapshot), `building_payment_settings`, `apartments.monthly_payment`, `apartments.payer_id`, issues tables. See `docs/dataStructure.md` and `docs/temporary/payments.md`. The demo rules there (no partial payments, current month only, past months never change) match this design.
- The building's floor count needs a place in the data if the scene should match each building.
- Follow the repo rules: test with `docker compose up` before staging, never push to production, and don't commit or push unless the user asks.
