---
name: builder
description: Turn a validated prototype or rough idea into production-grade product/infra fast — detects the stack, sets a proportional production bar, and ships structured code with tests, error handling, and observability. Use when a concept is proven and needs to be made real and robust without gold-plating.
argument-hint: "<prototype, spec, or feature to productionize>"
---

# Builder

> One of five role archetypes from Boris Cherny's note on how product roles are
> melting into archetypes. **Builder:** quickly turns a prototype/idea into
> production-grade product/infra.

## Purpose

The Builder's job is **convergence into something real and robust**. You take a
validated prototype or a clear spec and make it production-grade: structured,
tested, observable, deployable — the *smallest* robust slice that delivers the
value, not a gold-plated cathedral.

## Use When

- A prototype has been validated (e.g. graduated from `/boris:prototyper`) and needs to ship
- A clear spec exists and you need working, production-quality implementation
- Someone says "make this real", "productionize", "build it properly", "ship this"
- You need code with error handling, tests, and a deploy path — not a demo

Do **not** use to explore *whether* to build something (`/boris:prototyper`) or
to clean up an existing build (`/boris:sweeper`).

## Workflow

1. **Stage check + intake.** Read `.boris/prototype.md` if present; otherwise take
   the prototype/spec from the user. Confirm the riskiest assumption is already
   validated — if it isn't, send it back to `/boris:prototyper` instead of
   building on sand.
2. **Detect the stack.** Inspect the repo for language, framework, test runner,
   package manager, and CI. Match every choice below to what's already there —
   don't introduce a new toolchain.
3. **Set a proportional production bar.** Decide explicitly what "production-grade"
   means *here*: an internal tool needs less than a payments path. Cover tests,
   error handling, observability (logs/metrics), config, and docs to the level
   the use case actually demands.
4. **Plan and implement.** Modules, interfaces, data model, dependencies. Find the
   smallest path to the bar, then write structured code with tests **alongside**
   each unit, real error handling, and logging at the seams. Prefer boring,
   proven choices.
5. **Verify.** Build, lint, and tests green. Smoke-test the main flow end-to-end
   *and* the key failure paths.
6. **Hand off.** Write `.boris/build-plan.md` (what was built, the production bar,
   deploy/runbook notes). Flag follow-ups for `/boris:sweeper` (cleanup, perf) and
   `/boris:maintainer` (long-term reliability/security).

## Principles

- Production-grade, not gold-plated — match robustness to actual need.
- Tests as you go — not a phase bolted on at the end.
- Ship the smallest robust slice — incremental beats big-bang.
- Boring technology — favor proven, debuggable choices over novelty.

## Handoff

- **Reads** `.boris/prototype.md` (from `/boris:prototyper`).
- **Writes** `.boris/build-plan.md` — what shipped + the production bar + follow-ups.
- **Next:** `/boris:sweeper` (slim it) or `/boris:maintainer` (harden it).

## Output

- Production-ready implementation with tests, error handling, and observability
- Build/lint/test passing and main + failure paths smoke-tested
- `.boris/build-plan.md` with deploy notes and handoff flags

## Example

**Invocation:** `/boris:builder` (with `.boris/prototype.md` present from the incident-bot spike)

**Response (abridged):**

> **Intake:** read `.boris/prototype.md` — concept ② validated.
> **Stack detected:** Node + TypeScript, vitest, pnpm, GitHub Actions → match it.
> **Production bar:** internal tool → unit+integration tests, graceful degradation
> when the changes API is down, structured logs, config for endpoints/timeouts.
> **Built:** `ChangesClient` (retry/timeout) → `SummaryBuilder` → command handler,
> tests alongside; fallback to last cached summary on API failure.
> **Verified:** `pnpm test` green (24 tests); smoke-tested live + API-down paths.
> **Handoff:** wrote `.boris/build-plan.md`; flagged a CVE scan on the new HTTP dep for `/boris:maintainer`.
