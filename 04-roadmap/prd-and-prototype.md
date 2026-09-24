# Spotlight Curated Rail, Simplified PRD (StreamLine)

**Author:** Me · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Casual Browser

## 1. The Big Picture
- **Vision:** Every StreamLine subscriber finds something they'll love within seconds of opening the app, without ever needing to search.
- **Press release:** Today, StreamLine launched Spotlight, a hand-picked selection of titles that sits at the top of every subscriber's homepage. Too many viewers open StreamLine with an evening to fill and no idea what to watch. They scroll, try a search, give up, and close the app. Spotlight ends that routine. Instead of relying on an algorithm that often repeats what viewers have already seen, StreamLine's editorial team picks a small set of titles worth watching right now. Each one is checked to play in the viewer's region and suitable for their profile, so the first thing they see is something they can start immediately.

"Our subscribers told us the hardest part of StreamLine was deciding what to watch," said John Smith, VP, Marketing at StreamLine. "Spotlight gives them a confident answer the moment they arrive, so the evening starts with a great title instead of a search bar." Spotlight is available today for subscribers in [launch market], with more ways to discover titles that match each viewer's taste coming later this year.
- **Success metric:** Month-over-month subscriber retention rate
- **Guardrail:** total weekly viewing hours per subscriber

## 2. The Details
### User stories
- As a Casual Browser, I want strong picks shown the moment I open the app, so that I don't have to search to find something worth watching.
- As a Casual Browser, I want a short reason why each pick is worth my time, so that I can decide quickly instead of scrolling.
- As a Casual Browser, I want to start a Spotlight title in as few taps as possible, so that my evening starts with watching, not deciding.
- As a Casual Browser, I want the rail to skip titles I've already finished, so that every slot offers something new.
- As a parent using a kids' profile, I want Spotlight to show only age-appropriate titles, so that I can trust what appears at the top of the screen.
- As a StreamLine editor, I want my picks to fall back to the standard rail when they can't be shown, so that the homepage never looks broken.
### Screens to build
- Entry point: Homepage with Spotlight rail. The Spotlight rail is the first row, above the fold, with 6–10 title cards. A profile switcher (adult or kids) and an A/B toggle (Spotlight or control) sit in a small prototype control bar.
- Feature core: Title focus panel. Selecting a card opens a panel with artwork, title, runtime, rating, the one-line editorial hook, and a primary "Play" button.
- Success/confirmation: Playback started. A "Now playing" screen with the title and a mock progress bar, plus an on-screen event log showing the tracked events for the session.
### Functional requirements
- The Spotlight rail is fully visible without scrolling at 1280×720 (desktop) and 390×844 (mobile).
- The rail shows at least 6 and at most 10 titles.
- 0 titles marked unavailable in the user's region appear in the rail.
- On a kids' profile, 0 titles rated above the profile's maturity limit appear in the rail.
- When fewer than 6 curated titles pass the availability, maturity, and watched checks, the rail is replaced by the fallback algorithmic rail in the same slot, with no empty state shown.
- A user can go from homepage to playback in 2 taps or fewer (card, then Play).
- Titles marked as fully watched on the current profile never appear in the rail.
- Each of 4 events is logged exactly once per occurrence: rail impression, card select, play start, and exit without play.
### Smart behaviors (Situation → Outcome)
- If a curated title is unavailable in the user's region, then it is skipped and the next valid title moves into its slot.
- If the active profile is a kids' profile, then titles above its maturity limit are filtered out before the rail renders.
- If a title is marked fully watched on the current profile, then it is removed from the rail.
- If fewer than 6 curated titles remain after filtering, then the fallback rail renders in the Spotlight slot and is labelled as the fallback in the event log.
- If a title appears in both the Spotlight rail and another homepage rail, then it is shown only in Spotlight.
- If the A/B toggle is set to control, then the Spotlight rail is hidden and the standard algorithmic rail takes the top slot.
- If the user closes the focus panel and leaves the homepage without pressing Play, then an "exit without play" event is logged.
- If a title has no editorial hook, then the panel shows the standard synopsis instead of an empty line.
### Technical constraints
- Single-file React component, with state handled through useState only (no Redux, context, or reducers).
- All data is hardcoded mock JSON: curated list, fallback list, profiles, and watch history.
- No APIs, no database, no authentication, and no network requests of any kind.
- No real video playback. The success screen uses a static image and a mock progress bar.
- No localStorage or other browser storage. State resets on refresh.
- No routing library. The three screens switch through a single state variable.
- No editorial tool UI. The curated list is a constant array edited in code.
- No recommendation or ranking logic. The fallback rail is a fixed mock list.
- Analytics shown in an on-screen event log only, with no external tracking.

## 3. The Logistics
### Features out
- Per-user personalization of the rail. Belongs to A5 (Next). Mixing it in would blur the test of whether human curation works on its own.
- Curator names or profiles. Belongs to A7, which is cut.
- Hidden Gem badges on rail titles. Belongs to A3 (Later), and it adds visual noise to the first row users see.
- Thumbs up/down feedback on rail items. Play starts already tell us what works, so this adds data work with no payoff in this release.
### Edge cases & safety guard
- Unhappy paths
- Every curated title fails the availability check, so the fallback rail takes the slot and the event log records the fallback.
- A kids' profile leaves fewer than 6 eligible curated titles, so the fallback rail renders and is filtered by the same maturity rule.
- The curated list contains the same title twice, so it appears once and the next valid title fills the gap.
- Artwork fails to load, so the card shows the title name on a solid background rather than a broken image.
- A very long title name is truncated with an ellipsis on the card and shown in full in the focus panel.
- The user switches profile while the focus panel is open, so the panel closes and the rail re-filters for the new profile.
- The A/B toggle changes mid-session, so the top slot swaps immediately and the event log marks which variant each event came from.
- The user opens the focus panel for a title and then marks it watched in the mock history, so the title leaves the rail when the panel closes.
- What it must never do
- Never show a title above the profile's maturity limit, in the curated rail or the fallback.
- Never show a title that can't play in the user's region.
- Never render an empty, half-filled, or broken Spotlight slot.
- Never start playback without the user pressing Play.
- Never show a fully watched title in the rail.
- Never write profile names or other personal details into the event log. Profiles are recorded by type (adult or kids) only.
### Decision log
- 1. Curated list lives in code, not an editorial tool
- Decision	The curated titles are a constant array in the prototype.
- Rejected alternative	A simple admin screen for editors to pick and order titles.
- Why	The prototype tests whether curated picks stop Casual Browsers from abandoning. An editor screen tests something else and would take the designer's time away from the three user-facing screens.
- Revisit when	The rail passes its evals and moves to production build.
- 2. Fallback rail is a fixed mock list, not a real algorithm
- Decision	The fallback is a hardcoded list that passes through the same filters as the curated rail.
- Rejected alternative	A simple scoring or ranking function to imitate the production algorithm.
- Why	The prototype needs to prove the fallback appears correctly and safely, not that it recommends well. Ranking logic also drifts toward A5's personalization work.
- Revisit when	A5 enters its sprint.
### Evals
- Target: Accuracy
- Measure: Share of rendered rail titles that pass all three checks (availability, maturity, watched) across a scripted test matrix of adult and kids' profiles, region settings, and watch histories. Also the share of expected events that appear correctly in the event log.
- Pass Bar: 100% of rendered titles pass all checks, and at least 95% of expected events are logged correctly with the right variant label.
- Target: Time-On-Task
- Measure: Median time from homepage load to play start in moderated sessions with 5–8 Casual Browsers, compared with the control rail.
- Pass Bar: Median of 20 seconds or less, and at least 30% faster than control.
- Target: Safety
- Measure: Count of mature titles shown on kids' profiles across every test case, including fallback and mid-session profile switches.
- Pass Bar: 0 violations. Any single violation fails the prototype, regardless of other results.

## MoSCoW scope
- **Must:** Rail placement above the fold on the homepage; Lightweight editorial tool to pick and order titles
- **Should:** Hide already-watched titles; One-line editorial hook per title
- **Could:** Trailer preview on focus
- **Won't (now):** Per-user personalization of the rail

---
**Builder hook:** Build a working prototype based on this PRD. Use the User Story as the core flow, Functional Requirements as build constraints, and prioritize speed and clarity over visual complexity.
