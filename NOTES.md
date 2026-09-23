# LSCD Calendar — Simple Variant (Option 2)

**Built:** 2026-08-31 · **Deliverable:** `index.html` — the second of two
comparison links for the client, alongside the untouched original at
`builds/lscd-calendar`.

## Brief, and the correction that led here

First attempt (`builds/lscd-calendar-showcase`, now taken down — Pages
disabled + repo archived, no `delete_repo` scope on the CLI token to remove
it outright) read "picture format first" as a static screenshot of the
calendar UI up top. Rejected: *"this is not what I was looking for ... we
want a simple version of it ... not a picture of the calendar itself."*

Follow-up clarification, in Javier's words:
> "It should just feel like a calendar. They just want a simple version of
> that right now. It's like the first thing they look at is the schedule,
> when it should be the calendar first, right? Like our weekly schedule in
> a very simple way, and then maybe at the bottom, we can have stuff like
> the Tuesday schedule and the weekly class."

Read as: still a real, working calendar (not a photo, not a static
one-pager) — just fewer controls, and the *grid/at-a-glance* view should be
what loads first, with the day-by-day list ("Tuesday schedule") as a
secondary section further down the page, not the default landing state.

## What's actually different from `builds/lscd-calendar`

Same `SCHEDULE`/`EVENTS`/`SEASON` data and same visual tokens. Structural
changes only:

1. **No Studio/Style filter chips anywhere.** The original's `.filters` row
   is gone entirely. Each class card/row still shows its style as a small
   tag and its studio letter, so no information is lost — just the
   filtering UI.
2. **"This Week At A Glance" is the default, top-of-page view** — the full
   six-day grid (same component as the original's "Full Week View"), always
   visible, no toggle needed to reach it.
3. **"Browse By Day" is a separate, secondary section below it** — day
   buttons (Mon–Sat) plus the single-day detailed list. This is the
   "Tuesday schedule" Javier described. Defaults to today.
4. **School Year tab is one view** — just the month grid with prev/next
   arrows, defaulting to the current month. The original's Month/Full
   Season toggle and the full editorial timeline are gone; this variant
   only shows the calendar grid.

Tab label renamed "Weekly Classes" → **"Weekly Calendar"** to keep the
"calendar first" framing explicit in the UI itself, not just the layout
order.

## Verification

Headless Chromium (`playwright-core` + cached `chrome-headless-shell`),
screenshots at 1440/1280/375px: default Weekly Calendar view, School Year
month grid, a day-button click (Wednesday), and mobile. Zero horizontal
page overflow at every size (the at-a-glance grid still scrolls
horizontally *inside its own wrapper* on narrow screens, matching the
original's established mobile behavior for the same six-column grid).

## Deploy

Own repo, same pattern as the others: `javmartz04-ship-it/lscd-calendar-simple`,
`index.html` at root, GitHub Pages on `main`/`/`.
Live: https://javmartz04-ship-it.github.io/lscd-calendar-simple/

## Revision 2 — "make it look like a calendar, not a list"

2026-08-31, same day. Feedback after seeing the shipped version:
> "Can we make it like a calendar picture format? Like where they can see
> if it was a calendar instead of like a list?"

The "This Week At A Glance" section was a card stacked per day column —
correct information, but it read as six parallel lists, not a calendar.
Replaced it with a real time-grid: hour rows down the left (computed from
the actual data's earliest/latest start times, not hardcoded) × day columns
across the top, each class as a small chip in its start-hour's cell. Studio
letters double as the de-facto overlap lanes (a studio only runs one class
at a time, so chips in the same cell never collide) — multiple simultaneous
classes just stack as 2–4 chips in that hour's cell, still legible.
Saturday's morning classes and the weekday evening classes now visibly
occupy different rows of the same grid, which is real information (no
class ever runs at, say, 3pm any day — the grid shows that gap honestly).
"Browse By Day" below is unchanged.

Did not touch `builds/lscd-calendar` (the "regular way" option) — this
grid-first weekly view exists only in this simple variant.

## Revision 3 — picture preview on top, correctly this time

2026-08-31, same day. Final clarification, in the client's own written
words (no longer dictated, unambiguous):
> "When someone first opens the page, they want to immediately see the
> full calendar displayed visually — almost like a calendar image or
> preview. Then, underneath that calendar preview, visitors can access the
> interactive options... Basically: calendar preview at the top,
> interactive calendar/features below it."

This is the picture-first structure from the very first attempt
(`lscd-calendar-showcase`) — the thing that got rejected then wasn't the
*structure*, it was that the preview images were screenshots of the old
list-style weekly view. Now that the weekly view is a real time-grid
calendar (Revision 2), the same picture-first structure is correct.

Added, **in place on this same build/link** (no new repo — this is still
Option 2, not a third option):
- A `.preview` hero section before `#cal`: LSCD script wordmark, season
  eyebrow, an emphasis-line headline ("Your whole season, *one calendar*."),
  and two framed real screenshots — `assets/preview-weekly.png` (the
  time-grid "This Week At A Glance") and `assets/preview-year.png` (the
  month-grid School Year Calendar, August). Both captured from this same
  file with headless Chromium + PIL crop (see `system/LESSONS.md`), cropped
  to just their content (no duplicate header/tabs).
- A "Browse the calendar yourself ↓" link scrolling to `#interactive`
  (moved onto the existing `.cal-head`, no new element needed).
- The interactive tool below (grid + browse-by-day + month calendar) is
  completely unchanged from Revision 2 — same data, same behavior.
- Demoted the calendar section's `<h2 class="cal-title">` (was `<h1>`) so
  the page has one `<h1>` — the preview headline.

**If the schedule data changes**, the two preview PNGs go stale (same
caveat as the original showcase attempt) — regenerate them the same way:
screenshot this file's Weekly Calendar and School Year Calendar tabs at
1180×1200 @2x, crop from the `.section-head`'s y-position (weekly) or to
the `.section-divider`'s y-position (to exclude Browse-by-Day from the
weekly poster), replace the two files in `assets/`.

## Revision 4 — match the client's own schedule format

2026-08-31, same day. Javier shared a screenshot of the client's real,
currently-used schedule ("2026-2027 School Year Schedule Schedule" —
Studio A/B/C/D as rows, Monday–Saturday as columns, blue header bar) with:
> "I didn't like how it was designed at all ... doesn't look professional
> ... I give you a reference. I think that's how she's doing it now ...
> maybe something like this, you know, better?"

This is a different axis than everything built so far (all prior views used
day as the primary axis — tabs, day columns, or time rows). Rebuilt "This
Week At A Glance" as a real `<table>`: Studio A–D as row headers (shaded
panel column), Monday–Saturday as column headers (blue fill, matching the
reference's header bar), each cell a stack of that studio's classes for
that day — bold class name + NEW badge, muted meta line (ages · teacher ·
time), italic note. Removed the time-grid entirely (`.wk-*` CSS/JS) —
superseded, not layered on top of it.

Regenerated `assets/preview-weekly.png` from this new table (same crop
method as Revision 3) so the top-of-page picture preview matches what's
actually below it. `assets/preview-year.png` unchanged.

"Browse By Day" section stayed as-is below the table — never criticized,
still useful for a single-day read-out.

## Revision 5 — remove the picture preview

2026-08-31, same day. After seeing the studio-table version with the
picture preview on top: *"take off this whole part please"* (pointing at
the entire `.preview` hero section).

Removed the `.preview` section, `.preview-divider`, all associated CSS,
and the now-unused `assets/preview-*.png` files entirely. The calendar
section's `<h1 class="cal-title">` was restored to a real `<h1>` (it had
been demoted to `<h2>` to make room for the preview's own `<h1>`). Page
now opens directly on the interactive Studio × Day table — no static
picture step before it.

Net effect after five same-day revisions: Option 2 is the interactive
tool alone (studio-table weekly view + browse-by-day + month calendar),
no preview hero, no picture step. If a picture-preview concept comes back
into scope later, don't re-derive it from scratch — the removed CSS/HTML
is recoverable from this commit's history if needed.

## Lesson for `system/` (already folded in)

Voice-dictated feedback with crossed-out self-corrections ("that's like...
no wait") needs the concrete noun pinned down before building, not just the
adjective — "simple" and "picture" were both read wrong on the first pass
because I built from the adjective instead of asking what noun it modified.
See `system/LESSONS.md`.

## Studio review — 2026-09-02 (Loom, ~27:00–30:05)

Jess on the calendar: *"Yeah, I love it. I think it's very clean."* Two changes, nothing else:

1. **Script header matches the site.** *"Where it says Laredo school, if we can just keep the font consistent, and it's a brush script."* Great Vibes → **Hurricane** (the live site's script face), sizes bumped ~25% because Hurricane runs smaller.
2. **Class names shouldn't wrap like they're cut off.** *"Where it says grade three to four and pointe 3/4, like the 3/4 is not on Monday… it doesn't look like it has a second line, like it's incomplete. Will we be able to expand the columns?"* The very long Thursday class is fine to wrap: *"at least it reads like two classes."*
   - Container 1180 → 1400px, side gutter 40 → 24px at ≤1440px.
   - Each day column is **measured from its longest class name** (table-layout fixed + generated `<colgroup>`), not split evenly — Friday doesn't need Wednesday's width. A name over 230px is left out of the measurement and allowed to wrap, so "Preparatory Grade 2 / Grade 1 & Pointe 1" doesn't make Thursday huge and squeeze every other day.
   - Measured live, so it keeps working when class names change in the Sheet.
   - Result: at 1366px and wider only the Thursday class wraps. At 1280 some still wrap — there isn't enough physical width at that size.
   - On phones (≤860px) the table gets its full natural width, since it already scrolls sideways there.

**Scope flag, not built:** in the call Josh described the update flow as *"you export the PDF… I give you a little form, you upload it, hit submit, and it will auto update."* That's a PDF upload, not the Google Sheet Javier chose. Worth reconciling with Josh before the studio gets instructions.

**Heads-up for WordPress:** Elementor's default content width is 1140px. The embed needs a full-width section, or the wider columns get squeezed back.

## Font follow-up — 2026-09-17

After the review, Javier sent the studio's own MyFonts webfont kit: **Brush Script Std Italic** (Monotype, MyFonts build 3867246). That's the "brush script" Jess meant. The first pass had used Hurricane (the live site's script), which is thin and elegant; Brush Script Std is much heavier and matches the lettering in their logo. Shown side by side, Javier chose Brush Script.

- Font files are in `fonts/brush-script-std/`, loaded with `@font-face`. The fallback is `'Brush Script MT'`, the system version on Mac and Windows.
- All script sizes are about 16% smaller, because Brush Script Std sets wider than Hurricane at the same px. The line widths and layout stay the same.
- Licensing: the kit is licensed to the website owner (the studio). It's hosted in the public preview repos for now; long-term it belongs on their own server only.

## Second Loom review — 2026-09-17 ("Edits for Laredo Calendar and Registration Pages", 0:09–3:00)

Josh on the calendar: *"this is all perfect, I think this is actually great."* Four changes:

1. **Season line removed.** He couldn't trace "Season 16 · RISE · Rise Higher. Rise Together." to anything Jess sent (*"did we get that from somewhere that I'm not seeing… let's just remove this altogether"*). The `SEASON` constant went with it.
2. **Real logo at the top, linked home**, plus a Back to Home link (*"a little back to home link… it will go back to the actual main website"*). The logo is white artwork, so it sits on a black bar — the same trap noted in the client memory. The old blue script wordmark was removed rather than left duplicating the logo directly above it.
3. **"Schedule subject to change · Questions? Call" appears twice** — under *This Week At A Glance* and again centred just above the footer. **"Dance with Us. Grow with Us." removed** (*"take off the right dance with us grow with us altogether and just have this in the middle"*).
4. **The main site's footer rebuilt at the bottom** (*"our footers need to be all the same and congruent of the home page"*). Read off the live site rather than eyeballed: bg `#111`, Oswald 700 18px headings, 15px links at `rgba(255,255,255,.84)`, the stacked white LSCD mark at 190px, the script wordmark, tagline, three socials, quick links, contact block with hours, and the centred copyright bar.

**Links point at `violet-rail-831026.hostingersite.com`** (the build shown in the call). Josh flagged this himself: *"maybe we need to do the links afterwards."* They move when the real domain is live.

**Not ours, noted not done:** subdomains (`calendar.` / `register.`), DNS, and connecting both pages to Jess's GoHighLevel account. Manny has DNS access.

## Paste-proofing round — 2026-09-23

Javier pasted both pages into GoHighLevel and hit three things. All three had the same root cause: **a page builder strips the `<head>`**, which takes the charset declaration and any `<link>` with it.

1. **"A bunch of weird AI symbols."** Not stray characters — en dashes, em dashes, middots and the umlaut in GRÜV/CRÜ, decoded as Windows-1252 once the charset was gone. `ages 9–10` became `ages 9â€“10`, `GRÜV` became `GRÃœV`, `·` became `Â·`, and `→` became something that reads like a summation sign. Reproduced by serving the old file with `charset=windows-1252`, then fixed by escaping **every** non-ASCII character — HTML entities in markup, `\uXXXX` in scripts, `\NNNN` in CSS (entities are not decoded inside `<script>`, which is the trap). The rendered text was diffed before and after: byte-identical.
2. **Brush script gone.** The `@font-face` pointed at a relative path that does not exist on the host. Now embedded as a `data:font/woff2;base64` URI, so it cannot 404 wherever the block is pasted. Adds ~38KB to each page.
3. **Host CSS reaching in / footer looking wrong.** Built a hostile host page (Georgia serif, maroon headings, dashed-red tables, rounded buttons, `[hidden]{display:block!important}`) and diffed **every computed style** of every element against the clean render. 64 real leaks: maroon headings, dashed borders on the table, rounded tabs, and the `[hidden]` rule forcing the hidden tab panel open. Fixed by stating colour, border, font and radius explicitly on the rules that had been inheriting them, plus a scoped `[hidden]{display:none!important}`. Re-run: **0 leaks**.

**New file per build: `ghl-paste.html`** — the same page with no `<html>/<head>/<body>`, Google Fonts moved from `<link>` into an `@import` inside the `<style>`, and `<header>/<footer>/<nav>/<section>` written as `<div>` (with the CSS element-selectors rewritten to match) for builders that sanitise semantic tags. Generated by a script, so it never drifts from the real page.

**Answered, not actioned:** the subdomain question. No DNS or subdomain work was done from here — that has always been Manny's side.
