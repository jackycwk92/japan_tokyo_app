# Tokyo trip planner (9 to 14 Nov 2026)

A single-file, mobile-first trip companion for Jacky's Tokyo holiday, served by GitHub Pages from `main` / root. Everything lives in `index.html`: no build step, no frameworks, no external scripts, fonts or images. Keep it that way so it loads fast and works offline once opened.

## Trip facts (source of truth)
- Base: Cypress Inn Tokyo, Shin-Koiwa (新小岩). Flights: PR 530 / PR 422 KUL 02:20 to HND T3 14:10 (Mon 9 Nov); PR 421 / PR 529 HND T3 15:25 to KUL 01:15 Sun 15 Nov, both via Manila NAIA T1.
- Days: Mon Akihabara; Tue Asakusa, Komagome, Tokyo Station; Wed Lake Yamanaka + KABA (Fujikyu bus 09:20 out, 15:20 back, backup 18:50); Thu Tokyo Disneyland; Fri Shibuya then Shinjuku; Sat departure.

## Where things are in index.html (in order)
1. `<style>`: design tokens on `:root`, dark mode via `prefers-color-scheme` plus `[data-theme]`. Line colours are functional (JR signage), do not restyle them.
2. Main data script: `DAYS` (items: `k` = stop | eat | move; `fix:1` = fixed time that the live shift never moves; highway-bus journeys count as fixed too), `EAT`, `LINES`, `PHRASES`, `BOOKINGS`, `NEAR` (Nearby tab: places close to each day’s stops, not in the plan).
3. Chinese layer: `I18N` (UI strings), `ZH_DAYS` (per-day text; `items` array is **index-aligned** with `DAYS[day].items`), `EAT_ZH`, `HOURS_ZH`, `EATHRS_ZH`, `TRAINS_ZH`, `TODO_ZH`, `XFER_ZH`, `FC_ZH`, `AREA_ZH`, `MEAL_ZH`, `LINES_ZH`, `TAGS_ZH`, `NEAR_ZH`, `PHRASES_ZH`, `BOOKINGS_ZH`.
4. `HOURS`, `EATHRS`, `TRAINS` (first/last trains, key `day|fromJP|toJP`).
5. `TODO` + `TAGS` (things to do, keyed by the **English** stop title).
6. `XFER` (transfer walking guides) + the merge step that turns consecutive `move` items into one `journey`.
7. App IIFE: live shift (`shifts` in localStorage, `effMin`, `CLOSE` closing / last-order / last-entry times keyed by English title, “I’m here now” buttons on today’s plan), render functions, `FC` weather snapshot, `CHECK` (night-before lists), `RAIN` (rain plans), `LASTHOME` (last train home), live weather from Open-Meteo (only runs on github.io and similar hosts).

## Rules for edits
- **Every visible text change goes in both English and Chinese (Simplified).** If you add or remove an item in `DAYS[day].items`, add or remove the matching entry in `ZH_DAYS[day].items` at the same index.
- Keys in `HOURS`, `HOURS_ZH`, `TODO`, `TODO_ZH` must match the English stop title exactly. Restaurant keys match `EAT[].name` exactly (note the curly ’ in “Ban’nai”). `NEAR_ZH` keys match `NEAR[].name` exactly.
- Do not invent opening hours, train times or prices. Verify with a web search; if unconfirmed, write "Not confirmed, check Google Maps" / "未确认，请查 Google Maps" or prefix with ≈.
- Writing style: concise, British spelling, no em dashes (use commas, colons or restructure). Use ’ for apostrophes in visible text.
- Privacy: this repo is public. Never add booking references, passport, ID, card or account numbers, or phone numbers other than public emergency lines.
- Keep the page self-contained. No new external requests except the existing Open-Meteo weather call.

## Check before committing
- To test time-dependent features before or during the trip, add `?at=2026-11-09T15:00` (Japan time) to the URL; the clock shows “(sim)”.
- Open `index.html` in a browser at phone width (390 px), switch EN / 中文, tap through every day, the Eat, Nearby and Info tabs, and confirm the console has no errors.
- Commit to `main` with a short message; GitHub Pages redeploys in about a minute.
