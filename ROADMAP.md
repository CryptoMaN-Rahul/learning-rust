# Rust roadmap

Build order. Project 1 ports a tool Rahul already owns, so all effort goes to the language, not to inventing a spec. Projects 2+ are **chosen with Rahul based on what he wants to build toward** — not assumed. Fill them in once direction is set.

Check the box only when the **definition of done** is met: on PATH and used (or running and doing its job) **and** Rahul can explain what he wrote and re-derive it unaided. One project at a time. Timebox expires → ship, move on.

---

## 1. `extract` — archive extractor CLI  · timebox: 1 week
Port the `extract()` zsh function. Detects archive type, calls the right tool. Neutral starter, teaches Rust's core (`Result`/`match`/enums).
- [ ] `cargo new extract --vcs none` (this repo is already git — no nested repo), prints usage when run with no args
- [ ] match on file extension (`.zip`/`.tar.gz`/`.7z`/…) → dispatch
- [ ] real error handling: return `Result`, no `.unwrap()` in main path
- [ ] argument parsing with `clap`
- [ ] `cargo install --path .` → works from any directory
**Teaches:** `Result`/`Option`, `match`, enums, `std::process::Command`, error propagation with `?`.

## 2. `dedupe` — fast duplicate-file finder  · timebox: 2 weeks  · (CLI + systems/perf)
Scan a directory tree, find duplicate files by content, report wasted space. Rust's speed is the whole point here.
- [ ] walk a directory tree (`walkdir` crate)
- [ ] group candidate files by size, then hash to confirm real duplicates
- [ ] print duplicate groups + total wasted space
- [ ] parallelize the hashing (`rayon`) — the performance story
- [ ] benchmark with `hyperfine` against a Python one-liner doing the same
**Teaches:** iterators, `HashMap`, hashing, error handling, `rayon`/parallelism, borrowing across threads.

## 3. `axum` JSON API  · timebox: 2 weeks  · (web/backend — the stretch, do once ownership clicks)
Small API in Rust, once ownership stops fighting you. Reuse the URL-shortener idea, or a notes API.
- [ ] `axum` server with a couple of routes
- [ ] JSON in/out with `serde`
- [ ] shared state across handlers (`Arc<Mutex<…>>` or a pool)
- [ ] a store (in-memory first, then async DB)
- [ ] error handling that returns proper HTTP status codes
**Teaches:** `async`/`await`, `tokio`, `axum`, `Arc`/shared ownership, `serde`, lifetimes under async.

---

Direction is set: **CLI tooling · systems/perf · web/backend** (web is the later stretch). Swap in a better real project if one shows up — the rule holds: build something real, not a course toy.

## Log
_(append one line per session: date — what got done — next action.)_
- 2026-07-05 — workspace + toolchain set up — next: `cargo new extract --vcs none` and make it print usage
