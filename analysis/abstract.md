# Abstract

**Video:** How 1 Software Engineer Outperforms 138 - Lichess Case Study
**Author:** Tom Delalande
**Analyzed:** 2026-02-15

---

This video presents a case study of Lichess, a free and open-source chess platform serving
5.2 million games per day, maintained primarily by a single core developer, Thibault Dupire
(known as "TBO"). The study contrasts Lichess with Chess.com — a commercial competitor
with over 700 employees — to argue that small, focused teams using deliberate technology
choices can dramatically outperform large organizations.

The analysis examines Lichess's unconventional technology stack (Scala, MongoDB, Redis,
TypeScript, Snabbdom, Flutter, Rust) and its "monolith with satellites" architecture — a
pragmatic hybrid that avoids the overhead of full microservices while preserving the
flexibility to isolate high-load or high-risk components.

Beyond technology, the video distills a developer philosophy centered on radical simplicity:
treating code as a long-term liability rather than an asset, aggressively managing technical
debt, preferring minimal dependencies, and choosing tools that move complexity to compile
time. TBO's approach to UX over UI, his willingness to rewrite what isn't working, and his
embrace of open-source values are presented as the true drivers of Lichess's outsized impact.

The central lesson is that productivity at scale is less about headcount and more about
clarity of purpose, disciplined engineering, and a developer who treats their craft as
intrinsically rewarding — not merely as a means to financial gain.
