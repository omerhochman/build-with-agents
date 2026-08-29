# Handoff: bootstrap the ski app (a dip sibling)

**For:** the agent that will bootstrap the founder's ski-app idea in a new
repo, with access to `omerhochman/build-with-agents`, `omerhochman/dip`, and
`omerhochman/pointer`.
**From:** the agent session that bootstrapped dip (Aug 2026). This is a
one-shot brief — the bootstrap PR that consumes it should delete it from
this repo, per the template's bootstrap-deletes-itself ethos.

## The idea (all that has been decided — do not assume more)

The founder wants "a similar app for ski resorts or ski slopes" — a sibling
of dip (Swiss lake swimming, answered on open: one zero-click ranked
verdict). **That one sentence is the entire mandate.** Everything else —
name, resort-level vs. slope-level verdict, wedge region, season posture —
is undecided and belongs to the founder. A previous session drafted
interview questions (name grammar "short verb for the act itself" like
*dip*; resort-verdict vs slope-verdict vs powder-alerting; wedge region
candidates like the Zurich day-trip belt); the founder never answered them.
Re-ask in your own words; don't treat any prior recommendation as decided.

## Founder preferences (learned working together on dip — follow these)

- **Quality-gated, not date-gated.** Never put target dates or week
  estimates in docs; move as fast as quality allows; phases end when an
  explicit exit bar is met. (This was an explicit correction.)
- **Stamped-in-stone UX rule, verbatim in the firm calls:** *Beautiful UX —
  minimum information on the screen, maximum value.* Every screen reviewed
  against it.
- **Stay strictly in scope.** The founder was unhappy when an agent spent
  effort on work that was mentioned but not asked for. Do what was asked;
  offer next steps in one line; never start adjacent work unprompted.
- **Interview once, tersely.** One batch of numbered questions, each with a
  recommended default so one-word answers work. The founder answers
  minimally and expects you to take bold, recorded positions on everything
  below the escalation bar.
- **Delete before you add.** dip cut Redis from v0 (Postgres was enough) and
  replaced per-spot routing calls with an on-device heuristic. Prefer the
  version with fewer services, fewer secrets, fewer quotas.
- **Privacy as a structural claim, but never at performance's cost.** dip's
  ranking API takes a region key so location never leaves the phone — with a
  recorded condition that coarse coordinates are the sanctioned fallback if
  performance ever demands it. Same posture here.
- **Zero cost to the founder; no ToS-violating scraping, ever** (dip banned
  Google popular-times scrapers). Legal, attributable sources only.
- **Not a platform, a verdict.** No social, gamification, reviews, ads.
- Founder already has an **Apple Developer account** — never file a
  platform-fee decision, only access/credential asks when store items are
  reached.
- Repos stay **private** until the founder says otherwise (affects GitHub
  Actions minute budgets — dip runs ingestion in-process on the API host
  for this reason).

## Where to take what

**`omerhochman/build-with-agents`** — the skeleton. Fork/copy wholesale:
task loop + memory tiers (CLAUDE.md), `docs/AGENTS.md` protocols (delete the
Bootstrap section when done), `blocked-by-human.md` mailbox with its two
pre-seeded entries (stamp the new repo's URLs), self-skipping
`.github/workflows/ci.yml` (copy verbatim — it covers Expo + FastAPI as-is),
`.env.example` header, `docs/PROVIDERS.md` menu.

**`omerhochman/dip`** (branch `claude/dip-app-setup-ux-xjmt7j` until merged)
— the reference sibling; the closest product analog and the founder-approved
house style. Take the *shapes*, re-derive the *content* for ski:

- `CLAUDE.md` — firm-call structure and phrasing; inherit the stamped UX
  rule, zero-click answer, safety-is-a-gate-not-a-weight (for ski: avalanche
  danger, closed lifts, storm), honest-data-or-no-data ("Not today" state),
  legal-sources-only, verdict-not-platform, one-API-family.
- `docs/GUIDELINES.md` — the ten mobile-UX principles and the engineering/
  testing bars transfer verbatim; they are product-agnostic by design.
- `.claude/skills/` — copy the vendored `expo-design-system` +
  `expo-native-ui` skills, LICENSE, and provenance README as-is (re-check
  upstream for updates; provenance note explains how).
- `docs/ARCHITECTURE.md` — the patterns: exactly one endpoint family, never
  upstream from the device; freshness + confidence stamped at ingestion and
  carried to the UI; hard gates before scoring; fallback chains per signal;
  region = metro demand cluster (the ranking/cache/expansion unit); no cache
  layer until measured latency demands one; on-device travel heuristic.
- `docs/ROADMAP.md` — the structure to imitate (see "Roadmap instructions").
- `docs/PRIVACY.md` — the no-accounts, location-never-leaves-phone posture
  with its recorded performance condition.
- Entry style in `blocked-by-human.md` — decision briefs with options, one-
  line tradeoffs, and a recommendation; sub-minute numbered steps with full
  URLs for account actions.

**`omerhochman/pointer`** — engineering stack facts ONLY, no UI/UX
decisions: current Expo SDK pinned then `expo install --fix`; dev builds,
not Expo Go (`developmentClient: true` from day one); native dirs generated
by prebuild, never committed; TypeScript strict; `node --test` for pure
modules; the pure-model / gated-glue / one-consumer split (pure logic has no
framework imports and is unit-tested; IO glue degrades to a status, never a
crash).

## Research focus — do this BEFORE designing anything

dip's inputs were all clean open data. **Ski's are not, and one question
decides the whole product: is live lift/piste status ("X of Y lifts open")
legally machine-readable for Swiss resorts, and at what coverage?** Research
this first, with the no-scraping firm call as a hard constraint; if the
honest answer is "not without scraping", the verdict must be reshaped around
what IS legal (snow + weather + travel) rather than faked. Survey at least:

- **Snow:** SLF/WSL open data — IMIS measurement stations (snow depth,
  fresh snow) and the avalanche-bulletin API (a hard gate or display
  element, never a weight).
- **Weather at altitude:** MeteoSwiss OGD, open-meteo elevation-aware
  forecasts.
- **Lift/piste status (the hard one):** individual resort JSON endpoints
  and their ToS; aggregator ToS (bergfex, skiresort.info — expected
  scraping-hostile, verify); regional/cantonal OGD; whether
  opentransportdata.swiss GTFS/OJP feeds (which include mountain railways)
  expose operating status; OpenSkiMap/OSM for static piste geometry.
- **Travel time:** Swiss ski trips are transit-shaped — check the OJP /
  opentransportdata.swiss free tier before assuming a driving heuristic.
- **Crowding:** expect the dip conclusion (legal live signal is rare;
  time-of-day/holiday-calendar priors for MVP) — verify parking OGD or
  resort-published occupancy before settling.
- **Webcams:** Windy Webcams terms are already understood in dip's docs;
  check roundshot ToS as the ski-native alternative.
- **Prior art:** what existing ski apps license or scrape — it calibrates
  both feasibility and the open niche.

Record every source in the new repo's ARCHITECTURE with cadence, license,
gotchas, and risk — dip's "Ingestion" + "Source specifics & known traps"
sections are the format to match. Attribution requirements go in as a
roadmap item that blocks store submission, like dip's.

## Roadmap instructions

Build `docs/ROADMAP.md` exactly in dip's (post-restructure) shape:

1. Header states: quality-gated, no dates; one checkbox = one merged PR,
   startable without questions (name the library, API, quirk in the item);
   order IS priority; `⛔ see blocked-by-human.md` for blocked items; then
   name the product's real risk in one paragraph (for dip: data quality and
   score tuning — likely the same here, plus source legality).
2. v0 = one wedge region done perfectly, in **stages with explicit exit
   bars**:
   - **Stage 1 — walking skeleton**, ending with an installable app showing
     one real signal for real curated resorts from a deployed API, unstyled
     and honest, with ingestion live and banking history. Deploy is the last
     skeleton item and carries the hosting ⛔ (reuse dip's hosting decision
     brief — same options, same zero-cost tension).
   - **Stage 2 — data depth:** every score factor real or honestly labeled
     a prior; hard gates provably fire; deterministic (no-LLM) verdict-line
     generator; server-tunable weights.
   - **Stage 3 — experience:** design tokens FIRST (via the vendored
     expo-design-system skill), then the styled screens, the one signature
     interaction, meaningful haptics, the honest "Not today" state.
   - **Stage 4 — hardening:** staleness invariants, no-permission path,
     accessibility (VoiceOver verdict-first, Dynamic Type, color-never-
     alone), i18n scaffold (EN + DE in v0), one-sentence error posture,
     attribution screen, graduate the design-drift audit into CI,
     field-tuning loop (⛔ human field visits, in season), store submission.
3. v1.1 / v2 phases mirror PRODUCT.md's scope; a "Sequencing rationale"
   paragraph explains the order (data before beauty — a gorgeous screen
   over dishonest numbers is the unrecoverable mistake for a trust
   product) so later agents don't reorder it.
4. Season physics may shape *which* stage needs a live mountain (field
   tuning), never a calendar date. Note: ski season is winter — ingestion
   deployed early banks the coming season's history exactly like dip banks
   summer.

## Process

Follow this template's `docs/AGENTS.md` § Bootstrap: interview (one batch,
defaults), PRODUCT.md first, then the rest; one PR titled
`Bootstrap: <name>`; merging is the founder's sign-off. Creating the new
repo is the founder's account action — ask for a fork of
`build-with-agents` named after the product, via a blocked-by-human-style
numbered ask if they haven't created it already. Improvements you discover
that are template-shaped (not product-shaped) belong as a PR to
`build-with-agents`, so both siblings inherit them.
