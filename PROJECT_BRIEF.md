# Project brief — window-seat flight app

## What this is
An iOS app: scan your boarding pass before takeoff, get a personalized timeline of
what to look for out the window and when, then follow that timeline in the air with
zero connectivity required. Local notifications fire at the right moments
("look left in 2 min — Mount Fuji") even with WiFi off and no signal.

Working name so far: just "the window app" — no name has been locked in yet.

## Why this, why now
Researched an existing competitor, OutMyWindow, already live on the App Store with
real launch momentum (promo codes, an "AVCONLAUNCH" event tie-in, subscription
pricing). Their architecture is WiFi-first with an experimental GPS-only offline
mode as a secondary feature. This app inverts that: offline/precompute is the
primary mode, not a fallback. That's the core differentiation, not pricing or UI
polish alone.

Positioning: calm, anti-phone-scrolling. The pitch is "look up, not at your phone"
— flying as one of the few remaining screen-light stretches of time. This shaped
real product decisions (e.g. a glanceable notification + optional tap-through,
not a live map you're meant to watch the whole flight).

## Validation status — READ THIS FIRST
This is NOT a green light to start the full build yet. The plan was:
validate cheap before investing real engineering hours, because Kaiser lab
(30 hrs/week) and the Heelys project both have real claims on this summer's time,
and a 50–60 hour build competing with both is how side projects quietly die.

Validation step (landing page) is built but not yet live or posted:
- A complete `index.html` + `images/` folder exists (attached/uploaded alongside
  this brief). Pistachio/bamboo color theme (beige, sage green, white), Inter font.
- Email capture is wired to Web3Forms but the placeholder access key
  (`YOUR_ACCESS_KEY_HERE` in the `<script>` tag) still needs to be replaced with a
  real key from web3forms.com (free, no account, 2-minute signup) before the form
  actually works.
- Not yet deployed. Plan was GitHub Pages (free, plain HTML, no build step).
- Not yet posted anywhere. Plan was 2-3 relevant Reddit communities (r/flying,
  r/aviation, travel subs that allow self-promo posts), framed honestly as
  "thinking about building this, here's a mockup, what do you think" — not a hard
  sell, since that framing earns better engagement and feedback than a pitch.
- Success threshold agreed on: roughly 20 real signups within a week counts as a
  green light to invest real build time. A murky middle result (8–15) should be
  judged more by qualitative comments than the raw number.

**Next chat should help with:** finishing deployment (Web3Forms key, GitHub Pages
push), then drafting the Reddit posts, THEN — only after real signal comes back —
moving into actual app development.

## Product scope (v1, once/if validation passes)
Core loop: scan boarding pass → app fetches route, weather, landmarks while still
connected → generates an offline timeline → local notifications fire in-flight,
corrected by on-device GPS where available (GPS works passively without a data
connection on most aircraft, unlike WiFi).

Screens: scan/add flight, flight confirmation (fix misreads, confirm seat), full
timeline (pre-flight, doubles as a "preview before you fly" experience), flight
mode (minimal, in-flight, offline), settings (notification style, a "calm mode"
toggle).

Data plumbing decided on:
- Boarding pass parsing: IATA Bar Coded Boarding Pass (BCBP) format — existing
  open-source libraries exist for this, no need to write a parser from scratch.
- Flight route/schedule: OpenSky Network (free) for position; a great-circle or
  typical-routing approximation for the precomputed path is good enough for v1 —
  true historical-track precision is a v2 problem, not a launch blocker.
- Weather: Open-Meteo (free, no API key) sampled at waypoints along the route.
- Landmarks: a static, hand-curated or scraped database to start — keeps running
  cost near zero, avoids a live API dependency.
- Seat-side ("look left/right"): bearing of travel at each segment vs. seat
  letter — pure trig, no API needed.

Explicitly cut from v1: live map view, flight history/journal, account system,
server-side push (use local/on-device notifications instead — removes most
backend complexity and cost).

Cost profile: because there's no live per-user API dependency (weather/landmark
data can be cached/static), running cost stays near zero regardless of user count
— this is NOT like a live-tracking app's cost structure, so aggressive
monetization isn't required to stay sustainable. A free or near-free model is
genuinely viable here, not just a nice idea.

## Time/scope reality check
Rough estimate for a working v1 (not counting App Store review): 50–60 hours,
broken down roughly as boarding pass scan/parse (6–10h), flight + weather data
pipeline (6–10h), landmark database (4–8h), seat-side math + timeline generation
(4–6h), offline mode + notification scheduling — the trickiest part, hard to fully
test without an actual flight (6–10h), UI across 4–5 screens (8–15h), testing/bugs
(8–15h).

That's roughly 8–15 weeks at a "few hours here and there" pace — i.e., most of a
summer on its own, which is why this can't simply stack on top of lab + Heelys
without something giving way. Sequencing (validate first, build later, possibly
after the Heelys July push settles or once in Taiwan with a different time
rhythm) was the deliberate plan, not an accident.

## Content/voice notes for landmark facts
Notification copy should be short and directional ("look left, square dark spot
in the Northeast") — visual and instructional, not trivia. Save the deeper,
genuinely surprising fact (the kind that makes someone actually look up — e.g.
"this one mine supplies 40% of US coal" rather than population/founding-date
filler) for the tap-through detail screen. This distinction came from direct
iteration and is worth preserving as the actual editorial standard for any
landmark content added later, hand-written or generated.

## Assets included alongside this brief
- `index.html` — the validation landing page, pistachio theme, working layout,
  Web3Forms signup wired but needs a real access key.
- `images/01-scan.png` — scan boarding pass screen mockup.
- `images/02-notification.png` — lockscreen notification mockup (photorealistic,
  made externally, not by Claude — for reference/consistency only).
- `images/03-facts.png` — landmark detail/facts screen mockup (also made
  externally, photorealistic style).
- `images/04-timeline.png` — full pre-flight timeline screen mockup.
- `images/app-icon.png` — app icon, pistachio theme, plane window with mountains.

Note on visual consistency: two of the four gallery images (02 and 03) were
generated with a different, photorealistic tool, while 01 and 04 are flat
illustrated mockups built directly in code. They intentionally share the same
beige/pistachio palette and general layout logic, but are not pixel-identical in
rendering style. Worth keeping in mind if/when these get replaced by actual
in-app screenshots later — that's the point where true visual consistency
matters most, not at the mockup stage.
