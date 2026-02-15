# Key Takeaways

**From:** How 1 Software Engineer Outperforms 138 - Lichess Case Study
**By:** Tom Delalande | https://youtu.be/7VSVfQcaxFY

---

## The Core Insight

One deeply motivated, philosophically intentional developer, given the right tools and
freedom from bureaucracy, can build and maintain a product that rivals — and in some ways
surpasses — what 138+ engineers produce. Headcount does not equal output. Intentionality,
simplicity, and freedom from misaligned incentives do.

---

## Takeaway 1: Code Is a Cost, Not an Asset

> *"Lines of code are not valuable — they are a cost that is not paid while writing them,
> but while maintaining them, sometimes years later."*

Every line you add carries maintenance debt: compatibility, refactoring, bugs, library
migrations. The best code is no code. Ask: can this be done with *less*? The airplane
metaphor is useful — extra weight must earn its place.

**Actionable:** Before adding a dependency, feature, or abstraction, ask what problem it
solves and whether the long-term maintenance burden is worth it.

---

## Takeaway 2: Simplicity Is a System Property

TBO didn't just keep individual files simple — he chose an entire stack oriented around
simplicity: minimal frontend framework, strongly typed language that lets the compiler
carry complexity, a flat deployment script, a single-server architecture that avoids
distributed state management overhead.

**Actionable:** Design for simplicity at every layer. When something grows too complex,
that's a signal to investigate — not normalize.

---

## Takeaway 3: Technical Debt Must Be Actively Fought

TBO has almost no tech debt not because he's lucky, but because he had no product
manager telling him to ship instead of fix. He rewrote parts of the stack — even when
it took months — because unresolved debt compounds and eventually paralyzes.

> *"Don't live with tech debt — fix it before it slows you to a crawl."*

**Actionable:** Reserve time for debt reduction. Legacy code that no one wants to touch
is not normal — it is a failure mode. Don't normalize it.

---

## Takeaway 4: Strong Typing Is a Developer Force Multiplier

Scala's type system is the single most important technology choice Lichess made. It
allows for *"easy, carefree refactoring"* because the compiler catches errors before
they reach production. TBO's philosophy: move as much complexity as possible to compile
time, freeing the developer to focus on actual problems.

**Actionable:** If your language or framework doesn't give you confidence when
refactoring, that friction accumulates. Consider how much your type system is helping
(or failing) you.

---

## Takeaway 5: Use Frameworks Early, Libraries Later

TBO's biggest regret is adopting the Play Framework. It sped up early development but
Lichess outgrew it. The lesson: frameworks are helpful in early stages but can become
constraints as a project matures.

> *"Instead of a framework, I would now be looking at one library for HTTP, another for
> routing, another for HTTP forms and JSON, and so on."*

**Actionable:** Prefer composable, replaceable libraries over opinionated frameworks for
long-running projects. Design for the ability to swap components.

---

## Takeaway 6: Architecture Should Evolve From Monitoring, Not Dogma

Lichess's "monolith with satellites" wasn't designed upfront — it emerged from years of
watching what needed to scale and extracting only those pieces. There was no ideological
commitment to microservices or to monoliths.

> *"There was no master plan. We just looked at monitoring to figure out what needed
> scaling."*

**Actionable:** Don't adopt an architectural pattern because it's fashionable. Start with
the simplest thing that works and split only what observability shows you needs splitting.

---

## Takeaway 7: Right Tool for the Right Job

Lichess uses Scala for complex business logic (maintainable, expressive, safe) and Rust
for performance-critical small services (fast, low overhead). The choice wasn't
ideological — it was practical. Rewriting a small service in Rust is faster than
optimizing Scala to match Rust's performance.

**Actionable:** Keep a short list of tools you know well. For each new problem, ask which
of those tools is the best fit — and be willing to reach for something new when the
performance or correctness gap is large enough.

---

## Takeaway 8: The Frontend Is Dangerous Territory

JavaScript is inherently error-prone. TBO's philosophy is to keep the frontend as thin as
possible — move logic to the backend where it can be tested, typed, and controlled. He
chose Snabbdom (a virtual DOM with no extras) specifically to avoid the complexity that
comes bundled with larger frameworks.

**Actionable:** Resist the urge to put business logic in the frontend. Every piece of JS
you add is a maintenance and correctness risk.

---

## Takeaway 9: UX Is More Durable Than UI

Lichess looks plain. Intentionally. TBO invested in the feel of using the product — how
fast it responds, how intuitive interactions are — rather than aesthetics. Users return
to products that feel good to use, not just products that look good.

**Actionable:** Before working on visual polish, ask whether the core user flows are
smooth, intuitive, and fast. UX improvements compound; visual trends change.

---

## Takeaway 10: Craft and Joy Are Legitimate Career Strategies

TBO pays himself a modest salary by choice. He travels the world and works on something
he loves. The video challenges the idea that maximizing salary or status should be the
primary career goal for engineers.

> *"There's more to life than working at FAANG or selling your startup. There's so much
> joy to be had in working on your craft and making delightful products."*

**Actionable:** Evaluate your work not only by its market value but by the satisfaction
it generates. Intrinsic motivation is a productivity multiplier that no compensation
package can replicate.

---

## Takeaway 11: Open Source Changes Your Incentive Structure

Because Lichess has no investors, no ads, and no data-monetization model, every
decision optimizes for the user. This eliminates a category of perverse incentives that
affect commercial products. Open source isn't just a licensing choice — it's an
organizational commitment to a different kind of success metric.

**Actionable:** Consider who you're actually optimizing for at work. When incentives
align with user value, quality tends to follow.

---

## Summary Table

| Theme | Lesson |
|---|---|
| Code volume | Less is more; code is a liability |
| Type systems | Move complexity to compile time |
| Frameworks | Start with one, plan to outgrow it |
| Tech debt | Fight it actively; don't normalize it |
| Architecture | Evolve from data, not dogma |
| Language choice | Right tool for the job |
| Frontend | Thin as possible; UX > UI |
| Motivation | Craft and joy are real productivity drivers |
| Open source | Aligns incentives with user value |
