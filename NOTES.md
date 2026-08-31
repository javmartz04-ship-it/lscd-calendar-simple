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

## Lesson for `system/` (already folded in)

Voice-dictated feedback with crossed-out self-corrections ("that's like...
no wait") needs the concrete noun pinned down before building, not just the
adjective — "simple" and "picture" were both read wrong on the first pass
because I built from the adjective instead of asking what noun it modified.
See `system/LESSONS.md`.
