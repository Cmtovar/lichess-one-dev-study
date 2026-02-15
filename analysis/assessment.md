# Assessment: The Unspoken Practices Behind Lichess

**Video:** "How 1 Software Engineer Outperforms 138 — Lichess Case Study"
**Channel:** Tom Delalande | https://youtu.be/7VSVfQcaxFY
**Duration:** ~22 minutes
**Transcript source:** yt-dlp auto-captions (proper nouns corrected where identifiable)

---

## Thesis

The Lichess case study is not primarily about technology. It is about a cluster of
engineering practices that the industry rarely names, teaches, or incentivizes — practices
rooted in epistemic humility, deliberate constraint, and alignment between the builder's
values and the product's purpose. Thibault Dupire (known as TBO), the primary developer
of Lichess, achieves what 138 engineers at Chess.com cannot not because he is more
talented, but because his working conditions eliminate the structural causes of software
degradation: misaligned incentives, premature complexity, the social pressure to ship
over the pressure to be correct, and the accumulation of decisions made by people who
will not live with their consequences.

The video surfaces these practices, but mostly in passing. This assessment names them
directly.

---

## Context: What Lichess Is

Lichess is the second-largest chess platform in the world. It supports 5.2 million
chess games per day, offers engine analysis, puzzles, AI opponents, and game review —
all completely free, with no ads and no paywalls.

Its primary developer is one person. The full platform — backend, frontend, database,
infrastructure — runs on bare metal servers in France at an annual cost of approximately
$517,000. Chess.com, its commercial competitor, employs over 700 people. The presenter
estimates 138 of those are engineers.

The inciting example: during a high-profile cheating-accusation tournament, Chess.com's
platform showed a chess clock that was *off by over 30 seconds in a 3-minute game* —
a catastrophic precision failure — because brand-new laptops had timezone misconfiguration.
The presenter notes: *"one of the 138 engineers working at Chess.com should have been
able to wrangle these three systems together and create a working timer, especially since
the single engineer who built Lichess doesn't have this issue."* [transcript, lines 52-57]

That framing is rhetorical rather than rigorous — a bug at a tournament doesn't indict
a 138-person team. But it focuses attention on something real: organizational complexity
does not reliably produce better software. In some configurations it actively produces
worse software. Lichess is a working counterexample.

---

## I. Epistemic Humility as a Design Input

This is the most important practice in the video and the one almost no one discusses.

When TBO explains his choice of Scala, he says:

> *"Lichess is a big beast. We don't have a lot of manpower and I'm not the brightest
> star. We then need a lot of help from the language and compiler in order to scale in
> complexity. Scala gives us that."* [transcript, lines 140-145]

Read that carefully. He is not choosing Scala because it is impressive or fashionable.
He is choosing it because **he anticipates his own limitations and selects tools that
compensate for them.** This is the inverse of how engineers typically justify language
choices. Most justifications are capability arguments: "Scala is powerful," "Rust is
fast," "Go is simple." TBO's justification is a humility argument: I will make mistakes;
I need the compiler to catch them for me.

This reframes the entire stack discussion. Scala's strong type system, immutability
defaults, and functional paradigm are not intellectual preferences — they are a
substitute for the team members, code reviewers, and QA engineers he doesn't have.
The compiler is, functionally, a team member. It catches the classes of errors that
in a larger organization would be caught by a colleague in a PR review or by a tester
in staging. For a one-person team, a language that moves complexity to compile time is
not a luxury — it is a structural requirement.

The presenter states this consequence directly: *"the strong type system allows for
easy, carefree refactoring."* [transcript, lines 158-159] That word — carefree — is
doing a lot of work. It means TBO can restructure large parts of the codebase without
fear, because the compiler will surface every inconsistency before the code ships. This
is a form of psychological safety embedded in the toolchain rather than the team culture.

**What we don't say about this:** Most discussions of language choice are about syntax
preferences, ecosystem size, or hire-ability. The question of *what the language does
to your error rate as a solo operator* — how much cognitive load it offloads, how much
it protects you from your own future confusion — is rarely asked. TBO asks it explicitly.
The answer shapes every other technical decision he makes.

---

## II. The Absence of Organizational Friction as a Feature

The second unspoken practice is subtler: the video treats the absence of a product team
as a liability (TBO works alone, modest salary, long hours) but the evidence in the
transcript points in the opposite direction — the absence of organizational overhead is
one of Lichess's primary quality advantages.

On tech debt, TBO says:

> *"I didn't shy away from changing parts of the stack I didn't like, even when it took
> weeks or months. It's one of the perks of a project led by its developer. There was
> no one to tell me that something is more important than clearing away tech debt."*
> [transcript, lines 460-466]

That final sentence is crucial: *"There was no one to tell me that something is more
important than clearing away tech debt."* In virtually every commercial engineering
organization, there is always someone to tell you that. A sprint is ending. A roadmap
is committed. A demo is next week. The structural pressure in organizations is almost
always toward shipping, rarely toward correctness. Technical debt is the residue of that
pressure — the accumulated cost of decisions made under the wrong incentives.

The presenter contextualizes this: *"Tech debt is the biggest frustration for developers
according to the Stack Overflow developer survey this year, but Lichess has very little
of it. Since there is no product team, there is no one to push back on doing things
right the first time around, or going back and fixing past issues."* [transcript, lines
453-459]

The Stack Overflow survey finding is important to sit with. The biggest source of
developer misery, at scale, is tech debt. And the biggest cause of tech debt is
organizational pressure to defer quality work. Lichess is nearly free of it not because
TBO is a better programmer, but because the organizational structure that produces debt
does not exist.

**What we don't say about this:** "We need to reduce technical debt" is a common
organizational statement. It is almost never followed by a structural change that would
actually address the incentive creating the debt. Lichess demonstrates that the incentive
structure — not the individual — is the primary variable. You cannot solve a structural
problem by asking individuals to make different choices within the same structure.

---

## III. Architecture Designed by Evidence, Not Theory

The third unspoken practice concerns how architectural decisions are made and when.

On the evolution of Lichess's architecture, TBO says:

> *"The current state of things is a result of years of evolution. There was no master
> plan. We just looked at monitoring to figure out what needed scaling."* [transcript,
> lines 342-345]

This is the direct opposite of how architecture is taught. System design courses, job
interviews, and engineering blogs almost universally begin with a top-down plan: here
is the scale target, here is the SLA, here is the schema, here is the architecture that
supports those requirements. The plan precedes the evidence.

TBO inverts this. The architecture is a record of what actually needed to change, as
revealed by observation. Lichess's "monolith with satellites" pattern — a large central
monolith (Lila) with specific high-load or high-isolation services extracted as separate
servers — was not designed. It emerged from watching what the monitoring said.

The result is an architecture precisely fitted to actual load patterns rather than
hypothetical ones. Services that don't need to be independent aren't — and that
non-independence is an advantage, not a compromise:

> *"All the state is in one place and can be cached in heap for all modules of the site
> to use, which makes it very efficient and quick at runtime. Everything compiles as a
> single unit which ensures that the entire site is coherent and free of
> incompatibilities."* [transcript, lines 327-334]

The single server that looks like a limitation is actually what enables the heap cache
that makes the platform fast. A distributed system would require distributed cache
coordination — which reintroduces the consistency problems the single-server approach
avoids entirely. The constraint is load-bearing.

TBO is also honest about the limits of his perspective:

> *"I like where it's at, but to be honest, I'm biased by the fact that it's the only
> large codebase I've been working on for years, so I lack experience with different
> ways of doing things that I could compare it to."* [transcript, lines 345-350]

This is rare. A developer admitting that his architectural preferences may be shaped by
the limits of his own experience rather than by objective superiority of the approach.
That admission does not undermine the architecture — it models the correct epistemology
for architectural judgment.

**What we don't say about this:** Architecture is almost always justified after the
fact. Engineers are rewarded for sounding certain, for having a plan, for presenting
confident diagrams. The honest version — "we did this because the monitoring told us
to, and here is what we don't yet know" — sounds weak even when it represents better
epistemic practice.

---

## IV. Treating Frameworks as Time-Limited Contracts

The fourth practice concerns the lifecycle of framework adoption — something the
industry almost entirely avoids discussing.

On the Play Framework (the MVC framework Lichess originally built on):

> *"It made sense originally to speed up development with a cohesive and opinionated set
> of libraries that work together — a framework. But as years went and Lila got bigger,
> it outgrew the framework, and we would now be better off with smaller, independent
> libraries that we can swap as needed. So instead of a framework, I would now be looking
> at one library for HTTP, another one for routing, another one for HTTP forms, JSON,
> and so on."* [transcript, lines 170-181]

Frameworks are pitched as permanent foundations. Choosing a framework is described as
an architectural commitment, something you do after careful evaluation. What TBO
describes here is something different: frameworks have expiry dates. A framework that
is right at year one — because it eliminates hundreds of decisions at once — may be
exactly wrong at year five, because those eliminated decisions become locked
assumptions, and the product has outgrown them.

The alternative he now prefers — independent, composable libraries — is architecturally
more expensive to assemble initially and cheaper to maintain long-term, precisely because
each component can be swapped without touching the others. This is the Unix philosophy
applied to application structure: small tools that do one thing well and compose cleanly.

The frontend reflects the same principle. Lichess migrated from Mithril.js (already a
minimal framework) to Snabbdom (even more minimal — just a virtual DOM, nothing else)
when Mithril added complexity in a version upgrade. TBO's own words on Mithril:

> *"Oh my, did I love Mithril. It's such a breath of fresh air — virtual DOM done right,
> simple, and a beautiful API, fast at parse time and runtime. The love story lasted a
> couple of years. Then I realized that Snabbdom was pushing it all a bit further:
> simpler, smaller, faster. Mithril roll 2 improved performance but at the cost of
> complexity... I'm glad that Snabbdom didn't go that way. It makes it more stable and
> predictable."* [transcript, lines 258-269]

This is a developer who migrates *away* from something he loved when it moves in the
wrong direction. That requires a prior commitment to a principle (simplicity, minimal
dependencies) that is stronger than attachment to a tool. Most developers, once invested
in a framework, stay on it. The switching cost — rewriting, re-learning, convincing the
team — is too high. TBO pays that cost deliberately, repeatedly, because unpaid
complexity compounds.

**What we don't say about this:** Framework choices are discussed as one-time
architectural decisions. The question "at what point will this framework become a
constraint rather than an accelerant, and what will we do then?" is almost never asked
at adoption time.

---

## V. Code as a Liability, Not an Asset

The fifth practice is stated explicitly in the video but contradicts most engineering
culture so directly that it deserves careful treatment.

> *"Lines of code are not valuable — they are a cost that is not paid while writing
> them, but while maintaining them, sometimes years later. And they pile up. Adding
> lines of code to a program is like adding weight to an airplane. It better be worth
> it."* [transcript, lines 441-447]

And:

> *"With each new line of code, we're adding maintenance burden for years to come —
> compatibility with the rest of the site, with the mobile app, migrations through new
> libraries and language versions, refactoring the code itself, redesigning the UI. Each
> part of a new line of code will need to be revisited many times, adding time and risks
> to every code modification that affects it."* [transcript, lines 431-440]

Engineering output is almost universally measured in additive terms: features shipped,
story points completed, pull requests merged, commits pushed. The assumption embedded
in those metrics is that more code equals more progress. TBO's framing inverts this:
more code equals more future work, more surface area for bugs, more things that must
remain consistent as the system evolves.

The practical corollary is that deletion is productive work. Removing a feature, a
dependency, or a layer of abstraction that isn't earning its maintenance cost is a
positive contribution. This is barely legible within sprint-based delivery systems,
where deleting code generates no velocity points and closes no tickets.

The presenter connects this to a specific observed behavior: *"While many front-end
developers tend to add dependencies to try and solve issues, TBO is actively trying
to reduce them — even when the existing framework is one of the smallest frameworks
available."* [transcript, lines 292-296]

The mental model operating here is total cost of ownership for every line. Not "does
this code work?" but "does the value this code provides exceed its cumulative
maintenance cost over the lifetime of the project?"

**What we don't say about this:** Engineering culture rewards addition. There is no
"I deleted 2,000 lines of dead code" sprint story. There is no ceremony for the
dependency that was removed. The refactor that makes the codebase smaller is harder
to justify to stakeholders than the feature that makes it larger, even if the refactor
produces more long-term value. TBO sidesteps this entirely because he is his own
stakeholder.

---

## VI. Expected Fallibility as a Workflow Design Principle

The sixth practice concerns how developers relate to their own mistakes — not as a
psychological observation, but as a practical workflow question.

The presenter attributes this recurring statement to TBO:

> *"A common thing he says is that his job is to fix yesterday's bugs and create new
> bugs or features for tomorrow."* [transcript, lines 562-565]

This is usually read as self-deprecating humor. It is actually a workflow design
principle. TBO is not saying "I write bad code and feel fine about it." He is saying:
I expect to produce imperfect code; I design my process around detecting and correcting
errors quickly rather than preventing them completely. The corollary is that he invests
in observability, fast deployment cycles, and tooling (the compiler, the type system)
that surfaces errors early — because he has accepted that errors will exist and has
optimized for their rapid resolution.

This is functionally equivalent to what Google's SRE practice calls a "blameless
postmortem culture" — treating failures as information rather than as indictments —
but applied at the individual developer level rather than the organizational one. A
developer who treats bugs as expected outputs sets up better monitoring. A developer
who treats bugs as personal failures avoids deploying and avoids the tooling that would
surface their mistakes.

**What we don't say about this:** We talk about psychological safety at the team level
(blameless postmortems, no-shame retros) but almost never at the individual level: how
does a developer's relationship with their own fallibility shape their tooling choices,
their deployment frequency, their monitoring investment? TBO's answer is visible in
every technical choice he makes.

---

## VII. Salary as Alignment, Not Just Compensation

The final practice is the least technical and the most uncomfortable to discuss.

> *"He is an extremely hardworking programmer who could be making much more money by
> working on a for-profit project, but he chooses to work on Lichess and pays himself
> a very modest salary — because money is not important. He's happy doing what he loves
> and traveling the world."* [transcript, lines 472-478]

On open source as the product's core identity:

> *"There's no bigger goal to take your money or sell your data or even show you ads.
> Lichess is just a great product because building great products is rewarding for its
> author."* [transcript, lines 494-498]

Most discussions of compensation treat salary as a constraint to be maximized. TBO
treats it as a variable he can set to maintain alignment between his incentives and his
product's purpose. By taking less, he removes the financial pressure that would otherwise
compromise his decisions: the pressure to monetize, to grow beyond need, to take
investor money with its attendant expectations, to optimize for metrics that don't
correspond to user value.

This is not a poverty mentality. It is a structural choice with consequences. Because
Lichess is funded by donations and TBO's salary is modest, every technical and product
decision can optimize purely for the user. There is no feature that exists to drive
subscription conversion. There is no dark pattern that exists to keep users
psychologically hooked. The product is good because goodness is the only success metric.

The presenter notes that TBO's salary is so low that most donations flow into platform
improvement rather than personal compensation. This closes the loop: the community that
benefits from the product funds its improvement, without any extractive layer in between.

**What we don't say about this:** Salary optimization is treated as a universal good
in tech career advice. The possibility that deliberately limiting your compensation
could align your incentives more closely with your work — and thereby improve your
output and your satisfaction — is almost never presented as a legitimate strategy.

---

## Summary: The Practices Named

| Practice | What We Say Instead | Why It Matters |
|---|---|---|
| Use tools to compensate for your own limitations | Use the best/most powerful tool | A developer who knows their weaknesses can engineer around them |
| Organizational freedom from shipping pressure is a quality variable | Hire good engineers | The incentive structure produces the behavior, not the individual |
| Architect from monitoring, not from theory | Design the system upfront | Hypothetical architectures are often wrong; observed ones are often right |
| Frameworks have expiry dates | Choose the right framework | A framework right at year 1 may be wrong at year 5 |
| Code is a liability; deletion is productive | Ship more features | Total cost of ownership per line is almost never calculated |
| Expected fallibility as workflow design | Write better code | Accepting failure enables better tooling, monitoring, and recovery |
| Salary as alignment mechanism | Maximize compensation | Reduced compensation can preserve decision integrity |

---

## Closing Observation

None of these practices require a Lichess-scale project to apply. They require a
willingness to ask different questions: not "how do we ship faster?" but "what are we
accumulating that will slow us down?"; not "which framework is best?" but "when will
this framework become a constraint?"; not "how do I justify this refactor?" but "who
owns the mandate to do this, and what would happen if no one does?"

TBO's working conditions are unusual — he is his own product manager, his own CTO, his
own user — but the practices he embodies are not unique to solo operation. They are
legible, nameable, and in some cases teachable. What they require most is the habit of
asking what a decision costs over time, not just what it produces right now.

That habit is what the industry consistently undervalues. Lichess is what it looks like
when someone builds it anyway.
