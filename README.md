# Lichess One-Dev Study

Analysis of the YouTube video:
**"How 1 Software Engineer Outperforms 138 — Lichess Case Study"**
by Tom Delalande

→ https://youtu.be/7VSVfQcaxFY

---

## Contents

### [`transcript/`](transcript/)
- [`source.md`](transcript/source.md) — Video metadata
- [`transcript.txt`](transcript/transcript.txt) — Cleaned transcript (from auto-captions)
- [`raw_captions.vtt`](transcript/raw_captions.vtt) — Original VTT file from yt-dlp

### [`analysis/`](analysis/)
- [`abstract.md`](analysis/abstract.md) — ~200 word summary of the video's argument
- [`outline.md`](analysis/outline.md) — Full structured outline with comparison tables
- [`takeaways.md`](analysis/takeaways.md) — 11 key lessons with quotes and actionables

---

## The One-Line Version

A single developer, Thibault Dupire, built and maintains Lichess — a platform serving
5.2 million chess games per day — by prioritizing radical simplicity, fighting technical
debt obsessively, and treating code as a long-term liability rather than an output metric.

---

## Quick Reference: Lichess Stack

| Layer | Technology |
|---|---|
| Backend | Scala (Play Framework → migrating to libraries) |
| Database | MongoDB (considering PostgreSQL) |
| Cache/IPC | Redis |
| Frontend | TypeScript + Snabbdom |
| CSS | SASS |
| Mobile | Flutter |
| Performance services | Rust |
| Hosting | Bare metal, OVH, France (Ubuntu) |
| Architecture | Monolith (Lila) + satellites |

---

*Transcript extracted 2026-02-15 via yt-dlp. Auto-captions corrected where identifiable.*
