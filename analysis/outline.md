# Video Outline

**How 1 Software Engineer Outperforms 138 - Lichess Case Study**
*Tom Delalande*

---

## I. Introduction — The Chess Clock Incident
- Vladimir Kramnik accused GM José Martínez of cheating online
- Chess.com organized "The Clash of Claims" tournament to test the accusation
- Brand-new laptops opened each morning caused a chess clock to be off by 30+ seconds —
  a critical bug in a 3-minute game
- Point: 138 engineers at Chess.com failed to wrangle timezones on a countdown timer
- Lichess's single developer does not have this problem

## II. The Two Platforms Compared
| Attribute | Chess.com | Lichess |
|---|---|---|
| Size | Largest platform, multi-million dollar company | 2nd largest |
| Staff | 700+ employees, 138+ engineers | 1 core dev + 1 mobile dev |
| Features | Chess modes, engine review, puzzles, AI — mostly paywalled | Same features — all free |
| Funding | Commercial / subscription | Donations / open source |
| Repo | Private | Public |
| Monthly games/day | Not stated | 5.2 million |
| Annual cost | Not stated | ~$517,000 |

## III. Lichess — The Product and Developer
- Founded by Thibault Dupire (TBO) as an open-source hobby project
- 430+ contributors to the core repository; TBO is the dominant force
- Only his salary was paid by Lichess for years; today two salaries (TBO + mobile dev)
- TBO earns a "very modest salary" by choice — prioritizes craft over compensation
- Open-source identity is a core value: no ads, no data harvesting, no paywalls

## IV. The Technology Stack

### A. Scala (Backend Language)
- Strongly typed, expressive, high-level, functional
- Runs on the JVM — leverages the entire Java ecosystem
- Chosen over Haskell specifically for JVM compatibility
- Reduces boilerplate compared to Java; type system superior to Kotlin/Java (has union types)
- Supports currying, immutability, lazy evaluation, pattern matching
- Strong typing enables confident, safe refactoring
- Functional paradigms reduce side effects → code is easier to reason about
- TBO's quote: *"We need a lot of help from the language and compiler in order to scale in complexity. Scala gives us that."*
- **Regret:** Play Framework (MVC for Scala) — too opinionated; Lichess outgrew it
- Current preference: independent composable libraries instead of a monolithic framework

### B. MongoDB (Database)
- Document storage — stores data as JSON-like objects
- 5.5 billion games + ~15 billion other documents
- Serves tens of thousands of queries per second
- Fast, with good compression and seamless replication
- Self-hosted mitigates cost (MongoDB Atlas is expensive at scale)
- TBO's quote: *"If we had to remake that choice we would probably go for PostgreSQL"*
  — not because Mongo fails, but because PostgreSQL is fully open-source

### C. Redis (Inter-Service Communication)
- WebSocket service saves events to Redis cache
- Core service (Lila) reads from Redis
- Likely enables zero-downtime deployments: new version reads state from cache

### D. TypeScript (Frontend Language)
- Strong typing mirrors backend philosophy
- Compiler does heavy lifting → safe refactoring
- Paired with Snabbdom for UI

### E. Snabbdom (Frontend Framework)
- Minimal virtual DOM library
- Evolved from Mithril.js (also minimal) — chosen when Mithril added complexity
- Core module = virtual DOM only; all extras are opt-in modules
- TBO's quote: *"Mithril roll 2 improved performance but at the cost of complexity... I'm glad that Snabbdom didn't go that way."*
- Philosophy: take only what you need; avoid bundled features you'll never use

### F. SASS (CSS)
- Used for CSS management; TBO finds it annoying ("but that's just because CSS is annoying")

### G. Flutter (Mobile App)
- Android + iOS; the only part with a dedicated separate developer salary
- Migrated from Ionic/Capacitor to Flutter for better UX
- Chosen for open-source and multiplatform nature

### H. Rust (High-Performance Services)
- Used for smaller services with high performance requirements
- Practical right-tool-for-the-job decision: faster to rewrite in Rust than to optimize Scala

## V. Architecture — "Monolith with Satellites"
- **Main server (Lila):** Large monolith
  - All state in one place, cached in heap for all modules
  - Compiles as single unit → site-wide coherence
  - Runs on a single physical server (OVH, France)
  - Single server enables in-memory state → faster processing
  - Downside: single point of failure
- **Satellite services:** WebSockets, Stockfish analysis engine
  - Split out to deploy/fail independently without affecting Lila
  - Scala services (gray boxes) + Rust services (orange boxes)
- **Servers:** Managed bare-metal running Ubuntu (migrated from Arch Linux)
- **Deployment:** rsync + SSH scripts; "Manta" is the biggest machine for the monolith
- **Evolution:** No master plan — driven by monitoring, scaling what needs scaling
- TBO's quote: *"The current state of things is a result of years of evolution."*

## VI. Lag Compensation and Performance
- Chess games are sensitive to network lag (e.g., "flagging" in bullet chess)
- Lichess uses lag compensation + optimistic client-side rendering
- Works specifically because chess is turn-based
- Single server architecture simplifies this dramatically

## VII. Developer Philosophy

### A. Obsession with Simplicity
- *"Lines of code are not valuable; they are a cost."*
- Code is paid for not when written but when maintained — sometimes years later
- Adding code is like adding weight to an airplane: it better be worth it
- Fewer dependencies, features, lines, abstractions, patterns

### B. Tech Debt Management
- TBO's biggest differentiator: no product team pushing for features over quality
- Willingness to spend weeks/months fixing architecture issues correctly
- Quote: *"There was no one to tell me that something is more important than clearing away tech debt."*
- Don't live with tech debt — fix it before it slows you to a crawl

### C. Mistake-Ready Mindset
- *"My job is to fix yesterday's bugs and create new bugs or features for tomorrow."*
- Expects mistakes, plans to fix them — not self-flagellating, just pragmatic
- Set yourself up to fix bugs quickly rather than trying to write perfect code

### D. UX Over UI
- Lichess looks "bland" — intentional, not a resource constraint
- TBO is a programmer, not a designer; he makes up for it with UX focus
- Quote: *"I made up for my lack of UI skills by focusing on UX, and I think it paid off."*
- Be deliberate: don't add features without a logical place in the UI

### E. Frontend Minimalism
- JavaScript is "dangerous" — move as much logic to the backend as possible
- Keep frontend minimal; choose the smallest viable virtual DOM library

### F. Open Source as Identity
- Open from day one; first features born from community interaction
- No ads, no data selling, no paywalls
- Building great products is intrinsically rewarding

## VIII. Personal Takeaways (from the presenter)
1. **Simplicity** — great programmers do *less*: fewer deps, features, lines, abstractions
2. **Tech debt is dangerous** — fix it early, don't normalize it
3. **Frontend discipline** — minimize JS; UX > UI; be intentional about feature placement
4. **Database pragmatism** — robustness matters; everything else is preference; stick with it
5. **Right tool, right job** — Scala for complex logic, Rust for performance-critical paths
6. **Compile-time complexity** — static typing + functional paradigms = fewer runtime surprises
7. **Mistake-ready mindset** — ship, fix, improve; don't aim for perfection
8. **Lifestyle > status** — craft and joy beat salary and prestige

## IX. Closing
- Plug for Lichess Patreon (donation-funded, open platform)
- Presenter invites feedback on this new "case study" format
