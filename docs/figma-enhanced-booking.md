# Figma Enhanced: header colours, hairline and the booking bar

The owner, 2026-11-10, after the same changes on Figma PR: the header needs its line from the top of the page, and the
date bar is needed on Figma Enhanced too, in Figma Enhanced's own style. The mechanics are those of Figma PR's bar
(`docs/figma-pr-system.md`, "Booking bar"), in this edition's files and dress; the two editions share no code.

## Header colours (owner, 2026-11-10)

`src/lib/enhanced/styles/header.css`, `interactions.js` (`updateHeader`, `toneUnder`). The header always has a
background now; there are two, each in two strengths:

| | Words and logo | Background |
|---|---|---|
| 1 | white | dark green glass, 72% (`rgb(22 26 17 / 0.72)`) — at the top of a page, over a photograph or a film |
| 2 | white | dark green glass, 86% — after 40px of scrolling, over photographs, films and dark sections |
| 3 | black | white glass, 72% (`rgb(255 255 255 / 0.72)`) — at the top of a page that opens on text |
| 4 | black | white glass, 86% — after 40px of scrolling, over light sections |

The tone follows the section under the header, as before; the 72% at the top replaces the former full transparency
("twice as transparent" as the 86% glass). Copper words are gone: on white glass the words and the logo
(`/brand/logo-dark.svg`) are black and CHECK AVAILABILITY is the designer's green with beige words (owner, 2026-11-10;
until then black); on green glass they stay white, with the beige button.
The former dark brown state of an open mega panel is gone too: mega panels, the booking bar and its calendar open in
the header's glass of the moment, with its words' colour. A 1px hairline runs under the header — dark on white glass,
light on green glass — and the copper reading-progress line runs on it. The phone gradient over the opening picture
went with the transparency.

## The booking bar

`src/lib/enhanced/components/Booking.svelte`, styles `src/lib/enhanced/styles/booking.css`, wired in
`components/Header.svelte`.

- The designer's "CHECK AVAILABILITY" in the header is now the button that opens and closes the bar
  (`aria-expanded`); before, it opened Orbe's search page. The menu's CHECK AVAILABILITY still opens Orbe directly
  (the footer's left the footer on 2026-11-10).
- The bar lies over the page under the header in the header's glass of the moment (green with white words or white
  with black words, blurred, rounded at the foot like the panels), and unrolls: its height grows from nothing (grid
  rows 0fr → 1fr, 520 ms) while the fields settle in; closing rolls it back up. Closed, the bar is hidden and `inert`.
- Escape closes it and returns the focus to the button; opening a mega panel closes it (`enhanced:mega-open`); while
  it is open the header does not step away on scrolling (`interactions.js`, `updateHeader`).
- Fields: muted capitals for "CHECK IN", "CHECK OUT", "GUESTS" (the frames' eyebrow tracking) over values in EB
  Garamond on a hairline; guests 1 to 4 adults, 2 by default.
- "CHECK RATES" ("VER TARIFAS"), the same button as the header's CHECK AVAILABILITY in that glass, opens Orbe's results for the dates and
  guests in a new tab: `bookingResults()` in `src/lib/enhanced/site.ts` builds
  `https://reservations.orbebooking.com/Search/SearchAvailability/PAS6c/en/YYYYMMDD/YYYYMMDD/{adults}/0/` (`/es/` on
  Spanish pages). Without a check-out it asks for one and opens the calendar.
- On opening, the check-in is today and the check-out two nights later, unless the guest has chosen.

## The calendar

One calendar for both dates, in the header's glass, rounded like the panels, under the bar on the left.

- "Check in" or "Check out" opens it: two months side by side. The first click is the check-in, the second the
  check-out, without opening the calendar again; the calendar then closes and the focus moves to Guests. A click on or
  before the check-in while choosing the check-out starts again from that day.
- The two chosen days wear the buttons' colour, the nights between a tinted band, today underlined; while choosing the check-out the band follows the pointer. The nights are counted under
  the months ("2 nights" / "2 noches").
- Past days cannot be chosen; the calendar reaches 18 months ahead.
- Months and weekdays are in the page's language; weeks start on Sunday in English and on Monday in Spanish. Every
  day is a button whose label reads the whole date and whether it is the check-in or the check-out. Arrows move by day
  and week, Page Up and Page Down by month, Home and End to the week's ends, Escape closes the calendar; a click outside
  closes it too.
- With reduced motion the bar and the calendar appear without movement.

## Phones

The designer's header has no booking button below 761px (the menu and the page buttons open Orbe), so the bar opens
from 761px, where the calendar still shows two months.

## Spanish

The new words are in `src/lib/enhanced/content/es.json`, the same es-LA as Figma PR's bar: "Entrada", "Salida",
"Huéspedes", "2 adultos", "VER TARIFAS", "Agregar fecha", "Elige una fecha de salida.", "Mes anterior",
"Mes siguiente", "noche" / "noches", "fecha de entrada" / "fecha de salida". `scripts/spanish-strings.mjs` covers
them (989 strings).

## Checks (2026-11-10)

- Chromium, WebKit and Firefox, English and Spanish: the line is there at the top of light pages and photographs; the
  bar unrolls and rolls back; the header turns to glass with it; the calendar takes both dates in two clicks and
  closes; CHECK RATES fills Orbe's address; a mega panel closes the bar; Escape returns the focus to the button.
- axe (WCAG 2.2 AA): no violations at the top of the page, with the bar open, the calendar open and the menu open, at
  1440, 1024 and 800, in both languages.
- Layout: the 28 pages in both languages at 69 widths in the three engines, against the earlier runs.

## Buttons and the arch sections (owner, 2026-11-10)

- Every control of the edition is rounded 4px, as the designer's CHECK AVAILABILITY in the header: the token
  `--r-control` in `styles/tokens.css` (it was 12px) — the page buttons, the newsletter field and button, the
  next-page button, CHECK RATES and the calendar's days. The film's round "Watch the film" pill keeps its shape.
- The arch sections (Home, Experiences, The Vessyl Presents; `styles/films.css`): the title and its buttons stand in
  the middle of the arch's height instead of at its foot (the copy takes the arch's own top and foot insets and is
  centred), and the title is black. Phones keep the copy above the arch as before.
- Once the arch's film is open to the full screen it holds the screen for 20% of a screen's scroll before the page moves
  on (it was swept away the moment it opened; first 40%, then halved by the owner): the block is 205svh — 85svh open
  the arch, 20svh hold it
  (`interactions.js`, `updateArch`). The arch's title is 2px smaller than the hero size at every width
  (`calc(var(--t-hero) - 2px)`: 66px at 1440px).

## Panels, split photographs and the introduction (owner, 2026-11-10)

`styles/system.css`, at the level of each component:
- Overlay (a photograph with the translucent brown panel): the panel is rounded 4px; on phones, where it becomes a
  full-width band, it keeps square corners.
- Editorial splits (40/60, 50/50 and 60/40, either side): from 768px the photograph runs to the screen's edge on its
  side and is rounded 4px only at its inner corners; on phones, above the copy inside the margins, all four corners
  are 4px. The photo portals (the arch and the bowl) keep their shapes.
- Introduction (the symbol beside the opening prose): the symbol is about a quarter larger and darker (the designer's
  file is beige at half opacity; a CSS filter deepens it), the prose is 1px smaller, and `text-wrap: pretty` keeps a
  single word from ending a paragraph alone.
- The Dome photograph of Home's two overlays (`81536a0be9ccf3efc5a4`) leaned about 1° (its verticals ran to the right at
  the top). A levelled copy — turned 1° back and cut so that the Dome stands in the middle — is in
  `public/enhanced/photos/dome-level-{800,1200,1600,2016}.webp`, anchored to its top (`object-position: 50% 0%`) so the
  Dome's crown stays on wide screens. The original file is untouched: the frozen Figma edition, Figma PR and the menu
  thumbnails still use it.
- The drifting gallery (`styles/drift.css`): from 900px the cards stand against the screen's edges; their photographs
  have square corners at the edge and 4px corners towards the middle (the arrival mask keeps those corners), and the
  title and text under a left card align with the photograph's right edge, as they align with the left edge under a
  right card.
- Headings are black (`--heading-ink: #000` in `styles/tokens.css`) instead of the frames' green: every h1–h3 on light
  sections, the card titles, the description headings and the comparison rows. The copper accents stay copper (the
  eyebrows, the fact and rate headings the designer set in copper), headings on photographs stay light, and the green
  buttons keep their green (`--heading` still colours them).
- The opening mark (`components/Intro.svelte`, `styles/intro.css`) is black on white, as Blueprint's, instead of the
  designer's copper.
- The drifting cards are wider, towards the middle, in two steps: `--card: min(30vw, 55svh)` (first 27.5vw / 50svh) instead of `min(24vw, 44svh)`
  (432px instead of 346px at 1440px). They keep their places, so the square photographs close the vertical gaps by as
  much as they widen. Their captions keep the page's margin from the screen's edge.
- The next-page block (`styles/next.css`): the button stands in the middle of the picture instead of at its foot; it
  still arrives as the arch opens.

## The quote, the closing photograph, the footer, the patterns and the rising blocks (owner, 2026-11-10)

- The quote's symbol (`styles/system.css`, `[data-role="quote"]`) stands as in the designer's frame: its middle on the
  block's right edge, so its right half runs off the screen, and centred in the block's height, at most the block's
  height less its top and foot margins, so it is never cut at its top or foot.
- The closing photograph and the newsletter (`styles/closing.css`): from 768px the photograph's lowest 30% (`--nl-band`)
  is a vertical gradient that rises from the footer's colour, and the newsletter's words and form stand in it, at its
  foot, instead of in a band under the picture. The gradient holds the footer's colour over its lower 60% and eases out
  above, so every line keeps at least three quarters of that colour behind it: the small label keeps 4.5:1 against the
  brightest light of the photographs (measured on the Dome at night, the candlelit tables and the musician by
  lamplight). The photograph is `clamp(640px, 80svh, 900px)` tall, enough for the band to hold the words and the form
  from 768px to 2560px in both languages; between 768 and 1023px the words and the form stand side by side, the field
  above its button. On phones they stay under the photograph, which they would otherwise cover.
- The footer (`components/Footer.svelte`, `styles/system.css`), as on Figma PR: the five columns stand as one block in
  the middle (each column as wide as its longest line; one row from 1240px, where the Spanish fits with 32px gaps;
  three over two below; two columns on phones). The small wordmark, EN | ES and CHECK AVAILABILITY are gone; the
  Vessyl symbol, without the word, stands in the middle with the legal lines right under it. The footer's space above
  is half its space below (`--foot-pad`, the section spacing; later the same day `--foot-space`, below), measured from the foot of the closing photograph, where
  its gradient begins (on phones from the newsletter's button, since the form's empty message line takes no room
  there until it has something to say); the closing block adds no space of its own. Linked column headings take the
  headings' size on phones too.
- The pattern bands (the rail on Experiences, the events newsletter, the copper newsletter; `styles/rail.css`,
  `styles/shell.css`) use seamless tiles: the designer's 1440×800 panel, repeated, doubled the rows of fans at its
  joins. `src/lib/enhanced/patterns/direction-{forest,copper}-tile.svg` (outside the frozen `src/assets`) hold one 248-unit period of the panel's lattice (one
  fan, every other row shifted by half), drawn 95px wide — the former 550px-per-panel scale. The designer's panels are
  untouched; the frozen Figma edition still uses them.
- The rising blocks (`interactions.js`, `updateRise`): a block reaches the screen's edges once it covers 60% of the
  photograph, or three quarters of its own height when it is shorter than that. On wide screens a short description
  (Villas at 1920×1080 and 2560×1440) stopped at 90–93% of the width, a few pixels from the edges.

### Checks

- The newsletter band at 12 screen sizes from 768×1024 to 2560×1440 on five pages in both languages: the words and the
  form inside the photograph's lowest 30%, the gradient exactly 30%.
- The footer at 13 widths from 360 to 2560px in both languages: the block's left and right margins equal, no line
  wrapped. At 9 screen sizes from 360×760 to 2560×1440 on four pages in both languages: the space above exactly half
  the space below, the symbol and both legal lines centred.
- Every rising block on the 28 pages reaches the edges at 1440×900, 1920×1080, 2560×1440, 1024×768 and 390×844.
- The layout check now also reports a decorative picture (the designer's symbols and artwork, the footer's symbol) cut
  by the block that holds it at its top or foot or at a side inside the screen; it finds none in Chrome, WebKit and
  Firefox, and finds the quote's symbol when it is made to overflow.
- axe (WCAG 2.2 AA): no violations on Home, Villas, Eating here, The Vessyl Presents and Experiences at 1440, 1024, 800
  and 375, in both languages. Over the photographs, where axe cannot judge, the newsletter's lines were measured against
  the brightest pixels behind them at five screen sizes on four closing photographs: every line meets its ratio
  (the small label at least 4.7:1).

## Full-width photographs keep the page's rhythm (owner, 2026-11-10)

`styles/system.css`, "Full-width photographs keep the page's rhythm". Every block brings half the section step
(`--section / 2`, 53px at 1440px) above and below its content, so two blocks stand a whole step apart (106px). The
overlays (the Dome photographs with the brown panel), the galleries and the map fill their block to the edges, so their
own half went under the picture and their neighbours stood only half a step from them (53px, 32px on phones). They now
bring that half as an outer margin, so the words of the blocks around them stand a whole step from the picture, as
everywhere else. They still touch the designer's beige bands (Villas, Treehouse, Glamping Tents), other full-width
photographs and the page's ends (a gallery that closes a page runs into the footer); the foot of the Home photograph
that the drifting gallery rises over keeps no margin (step 2.5); and the gallery under the Frequency Dome's pinned change
block takes none, since that block centres its picture in a whole screen and already leaves more than a step.

Checked at 1440px and 390px on all 18 such blocks of the 28 pages: a whole step to every ordinary neighbour (106px and
64px). A large title block (Hero text only) keeps its own larger top space, as after any block (137px at 1440px).

## The menu, the header's button and the footer's spaces (owner, 2026-11-10)

- **The menu** (`components/Menu.svelte`, `styles/shell-edition.css`, "The menu's top row and height"). The top row
  follows Figma PR's: the close button on the left, where MENU opened the menu, in a 44px square with 4px corners (it
  was a circle on the right); the wordmark in the middle; the globe with EN or ES and CHECK AVAILABILITY ↗ on the right
  from 1024px, where they fit beside the wordmark (below, they stay at the foot of the menu, as before). The button keeps
  the designer's green fill and beige words wherever it stands, with 4px corners, as wide as its words. The preview
  photographs have 4px corners (they were 10px). The group headings and the links are 2px smaller at every width (35px
  and 20px at 1440px). The menu is more compact in height — a lower top row (80px, 72px on phones), less room under it,
  12px under the headings, 7px above and below each link, and the two rows of groups closer — so the six groups and the
  preview fit the screen from 1280×720 up (at 1440×900 it took 1,074px before). On phones the single column still
  scrolls.
- **The header's CHECK AVAILABILITY** (`styles/header.css`): on white glass it is the designer's green
  (`--surface-forest`) with beige words, as the menu's, instead of black; on green glass it stays beige, since green
  would not show there. CHECK RATES and the chosen days of the calendar wear the button's colour, so they turned green
  with it. An old rule from the Figma edition that made the button black on pages opening on text now takes the
  header's colour too (`styles/shell-edition.css`).
- **The footer's spaces** (`styles/system.css`, `--foot-space`): the space between the links and the symbol now equals
  the space under the legal lines, midway between the former two (7/8 of the section step: 92px at 1440px, 56px on
  phones; it was 81px and 105px). The space above the columns stays half the space below (46px at 1440px). The last
  link of a column no longer adds its own space on phones.

Checked: the menu fits the screen at 1280×720, 1366×768, 1440×800, 1440×900, 1536×864, 1920×1080, 2560×1440 and
1024×768 in both languages, in Chrome, WebKit and Firefox, with the close button on the left, the wordmark exactly in
the middle and the button unclipped; the footer's three spaces at 9 screen sizes on two pages in both languages; axe
(WCAG 2.2 AA) with no violations at the top of the page, with the booking bar, the calendar and the menu open, on a page
opening on a photograph and on one opening on text, at 1440 and 1024, in both languages.

## The map's film (owner, 2026-11-10)

On Where you sleep the aerial photograph of the map (`pages/P02.svelte`, "Map") is now the owner's short film: the
clouds part over the property (`public/enhanced/clips/map-reveal.mp4`, 1712×1080, 4s, 1.9 MB; `map-reveal-m.mp4`,
960×606, 0.7 MB, below 768px; re-encoded from `Vessyl-cloud-reveal-4s-v2.mp4` without sound and with the index at the
front). The film's last frame is the same place as the photograph, so the frame's crop and the names stay as they were.

- It plays once, silent and without controls, when a third of the block is on screen, and stays on its last frame
  (`interactions.js`, `setupReveal`; the attribute `data-reveal` keeps it out of the loops' logic). It loads only when
  the block is a screen away; until then its poster is the first frame, the clouds (`map-reveal-first.webp`).
- The names on the map are hidden for the film's first two seconds and come in over its last two, the fade timed to
  end with the film (`data-map-key`, `--key-fade` in `styles/system.css`).
- With reduced motion the film never plays: its poster becomes its last frame (`map-reveal-last.webp`) and the names
  stand at once. A change of language that rebuilds a map already reached shows it the same way, so the clouds do not
  part again.
- The figure carries the photograph's description (`role="img"`, `aria-label`); the film itself is hidden from
  assistive technology.
- The Home next-page block towards Where you sleep and the menu's small preview still show the photograph.

Checked in Chrome, WebKit and Firefox, at 1440×900 and 390×844: idle until the block comes on screen, playing at once,
ended at 4.0s on its last frame, the phone file on phones; the names at opacity 0 until 2.0s and 1 at 4.0s; reduced
motion and a change of language with the still frame and the names shown. axe: no violations on the page.

## The map's last frame elsewhere (owner, 2026-11-11)

The designer's aerial screenshot of the map (`0b6f7938d784334e4a7e`) also stood in two other places in this edition:
the next-page door at the foot of Home, which leads to Where you sleep (`next.ts`, door 2), and the menu's picture for
Where you sleep (`navigation.ts`, cover 2: the preview beside the menu's columns and its links' `data-cover`). Both now
show the owner's last frame of the film, the property with the clouds gone, as a still photograph (from his PNG,
1579×996, 3.8 MB):

- `public/media/7bb7e41047b306320263.webp` — the whole frame, 1579×996, 344 KB;
- `public/enhanced/photos/7bb7e41047b306320263-800.webp` and `-1200.webp` — the door's smaller copies, 110 and 225 KB
  (`photo-widths.json`);
- `public/media/thumbs/7bb7e41047b306320263.webp` — the menu's 640px picture, 70 KB.

WebP at quality 75, method 6, metadata stripped (`magick <png> -strip [-filter Lanczos -resize <w>x] -quality 75 -define
webp:method=6 <file>`): side by side at full size with the PNG and with quality 80 and 85 no difference shows, and the
whole frame weighs what the designer's 2400px screenshot did. The frame is narrower than the 2400px screenshot, so the
door's largest copy is the frame itself; its proportions are the screenshot's (1.585), so the door's crop is unchanged.
The screenshot's three smaller copies (`public/enhanced/photos/0b6f7938d784334e4a7e-{800,1200,1600}.webp`) served only
the door and are removed; the screenshot itself stays in `public/media/` for the frozen /figma/ edition.

Checked at 1440×900 and 390×844, in English and Spanish: the door draws the frame, the menu's preview shows it on
hovering Where you sleep, and no page of this edition refers to the screenshot any more.

## Content from the agency's brief (owner, 2026-11-11)

The text corrections made in Figma Edit against the agency's content brief (*The Vessyl — website content*, Wunderlich
Kaplan Communications, 3 October 2026) are carried over here, word for word and with the same Spanish
(`content/es.json`): see `docs/figma-edit.md` for the list and the rule (the brief's text takes priority over the
designer's; what the designer put in place of a placeholder takes priority over the brief's empty brackets). Nothing in
the design or the order of the blocks changes.

- Every page: the brief's title tag and meta description (`meta.ts`, used by the page route).
- Components: the logo's link is "The Vessyl home" (and alt "The Vessyl" where the logo carries one); the newsletter's
  button "Keep me posted" ("Avísenme") and the example address "you@example.com" ("nombre@ejemplo.com"); in the footer,
  The Dome among the experiences and "[Reservations email]" among the contact lines.
- Pages: the Dome button on Home; the other rooms' rate and "See the villa" on the places to stay; the treehouse's and
  the tents' placeholder facts and rate lines; the offers on Rates and offers; lengths and prices on the experience
  cards; prices a person on the Day and the Half Day; "no more" and "which most people use" on the Frequency Experience;
  For Two's hours and Dome fact; the minimum age on Horses and the River; the Hearth's labels; "Private movement or
  breath session"; El Rancho's outside reservations; "See El Rancho" on Moloka; "Download the retreat pack"; Arenal's
  "What is on this page" and airports in hours; San José by road and its transfer on Getting here.

**Page addresses** (owner, 2026-11-11): this edition is on the brief's addresses too, as Figma Edit (`/stay/`,
`/stay/villas/`, `/offers/`, `/experiences/day-pass/`, `/dining/el-rancho/`, `/buyouts/`, `/arenal/getting-here/` and so
on; `site-data.json`, the pages' links and the components' routes). The 19 former addresses, in English and under `es/`,
are prerendered as permanent redirects to the new ones (`formerSlugs` in `site.ts`, the page route's `entries` and
`load`), so links already shared keep working; GitHub Pages serves them as a page that moves on at once
(`<meta http-equiv="refresh">` with a script). The openings' films and photographs are keyed by the new addresses (`heroes.json`, `hero-photos.json`).

## Content brief, part 2: what words alone could not do (owner, 2026-11-11)

Done in Figma Edit, Figma Enhanced and Figma PR alike, with the designer's own blocks (`docs/figma-edit.md`):

- **The booking engine in the brief's three places** (the header, the hero on Home, the foot of the places to stay and
  the experiences). Rates and offers: the five offers lose their "Check availability" (with the header they made four
  on one screen), and the one button the brief puts on the page closes "What every rate includes". The Villas, the
  Treehouse and the Glamping Tents end, before the other rooms, on the designer's closing block (as on the
  experiences): "From [rate] a night" and the page's own booking button.
- **Take the whole property: the brief's five buyout items.** The designer's three cards and two more, "The land —
  The trails, the river and the horses." (horses on the trail, from Horses and the land) and "Someone on our side — Who
  runs the logistics so you do not have to." (a member of the team at the Hearth, framed at 50% 38%). From 900px they pass along the edges like the designer's three (the drift, `docs/figma-enhanced-step2.md`, 2.4): 0/45/90/135/180svh of a 240svh run, depths 1.2/0.8/1.2/0.8/1.2, sides alternating; from 768 to 899px two across with the fifth across both columns, its picture as tall as the others' (`styles/system.css`); the horses' photograph has its 800/1200/1600 copies in `public/enhanced/photos/`.
- **The recording studio: "Who has worked here"**, the brief's last section, in the designer's text block (eyebrow
  "Credits", the brief's heading, and the line "TO SUPPLY: artist and project credits, confirmed by the property, with
  each artist's permission.", as the Weddings page shows its paperwork until the property supplies it).

## Home's film (owner, 2026-11-11)

Home's opening clip is the owner's film of the property (`vessyl-home.mp4` on the Desktop, 61s, 1920×1080, 25 fps, with
sound, 51 MB), in place of the pool clip (`/bp/video/loop-pool`). Made the way this edition's clips are (speed, stage 3):
no sound track, H.264 High, `+faststart` so it plays while it loads, a key frame every 2s; for large screens 1920×1080,
x264 `-preset slow -crf 25 -maxrate 4500k` (28.8 MB), for phones (up to 767px) 1280×720, `-crf 27 -maxrate 2200k`
(12.5 MB); its first frame `vessyl-home.webp` (1920×1080) is the poster. The files are in `public/enhanced/clips/`
(`vessyl-home.mp4`, `vessyl-home-m.mp4`, `vessyl-home.webp`), published with Figma Enhanced and Figma PR only (Blueprint
publishes all of `public/bp/`, so the film is not put there).

- `pages/P01.svelte`: the opening's `<video data-film>` with the two sources and the poster; "Watch the film" still opens
  the brand film with sound.
- Warm-up under the opening mark: the film streams from the first moment. The head script (`clips-first.js`) starts the
  first screen's clip as soon as it can play, and the mark waits for it to be moving (`firstClipMoving`, at most 6s);
  `heroes.json` gives Home's first picture as the film's first frame, decoded under the mark. Measured on Home at 1440
  and 390, fast and on an 8 Mbit/s line: the film is asked for within 0.1s, and is playing (0.5–2s in) when the mark
  leaves.
- The next-page block that leads home (`next.ts`, door 1) shows the same film: `clip('vessyl-home')` gives a clip in
  `/enhanced/clips/` with its own source and first frame (`components/Next.svelte` takes `src` and `poster` when a clip
  has them, `/bp/video/<name>` otherwise).


## Horses and the land: the three cards' photographs (owner, 2026-11-11)

The frames gave Horses and the land's three cards (Groundwork, Trail ride, Short ride) the photographs of Sound and
movement's three cards (a massage, headphones on a cushion in the Dome, a group stretching in the Dome), a copying slip.
Each now shows a horse photograph from the property: Groundwork a woman standing with a white horse in the river
(`ea5d3eb36b2031e294f0`), Trail ride a saddled horse on a forest trail (`bb39a8510ce86bcd65fd`), Short ride two riders on
their horses in the shallow river (`55161e7692315fc7eac4`). Their copies for the screen's size are in `public/enhanced/photos/` (800, 1200, 1600), and their descriptions are written for this edition.

## The footer's last row (owner, 2026-11-11)

The symbol at the foot of the footer is the PR edition's size, 64px (it was 56px, 48px on phones), and the two lines are
set apart as in the Figma edition: "© The Vessyl 2026 • La Fortuna, Costa Rica" to the left and "Privacy • Terms &
Conditions" to the right, with the symbol in the middle (`styles/system.css`, `.footer-bottom` as a three-column grid).
The lines' letters stand on the symbol's lower edge: the row aligns its items' last baselines, and the symbol, a box
without text, takes its bottom edge as its baseline (checked in Chrome, WebKit and Firefox: the baseline and the
symbol's edge coincide). Below 768px, where the three do not fit in a row, the symbol and the two lines stand one under
another in the middle, as before.

The space from the last links to the symbol and from the symbol to the bottom of the page is half what it was, and the
two stay equal: 45/46px at 1440 (was 90/92), 38/39px at 1024, 27/28px at 390 (was 54/57).

