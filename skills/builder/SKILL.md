---
name: builder
description: Turn a validated prototype or rough idea into production-grade product/infra fast — structure, error handling, tests, observability, and deployment readiness. Use when a concept is proven and needs to be made real and robust without gold-plating.
argument-hint: "<prototype, spec, or feature to productionize>"
---

# Builder

> One of five role archetypes from Boris Cherny's note on how product roles are
> melting into archetypes. **Builder:** quickly turns a prototype/idea into
> production-grade product/infra.

## Purpose

The Builder's job is **convergence into something real and robust**. You take a
validated prototype or a clear spec and make it production-grade: structured,
tested, observable, and deployable — the *smallest* robust slice that delivers
the value, not a gold-plated cathedral.

## Use When

- A prototype has been validated (e.g. graduated from `/prototyper`) and needs to ship
- A clear spec exists and you need working, production-quality implementation
- Someone says "make this real", "productionize", "build it properly", "ship this"
- You need code with error handling, tests, and a deploy path — not a demo

Do **not** use to explore *whether* to build something (that's `/prototyper`) or
to clean up an existing build (that's `/sweeper`).

## Workflow

1. **Intake.** Read the prototype/spec. Identify the happy path that already
   works and the gaps that production requires: error cases, edge cases, scale
   limits, security basics, configuration.
2. **Define the production bar.** Decide explicitly what "production-grade" means
   here: tests, error handling, observability (logs/metrics), config management,
   docs. Keep it proportional — robust, not gold-plated.
3. **Plan the build.** Modules, interfaces, data model, dependencies. Find the
   smallest path from prototype to the production bar. Sequence the work.
4. **Implement.** Write structured code with tests **alongside** each unit, real
   error handling, and logging at the seams. Prefer boring, proven choices.
5. **Verify.** Build, lint, and tests green. Smoke-test the main user flows
   end-to-end. Confirm the happy path *and* the key failure paths behave.
6. **Handoff.** Provide deploy/runbook notes. Flag follow-ups for `/sweeper`
   (cleanup, perf) and `/maintainer` (long-term reliability/security).

## Principles

- Production-grade, not gold-plated — match robustness to actual need.
- Tests as you go — not a phase you bolt on at the end.
- Ship the smallest robust slice — incremental beats big-bang.
- Boring technology — favor proven, debuggable choices over novelty.

## Output

- Production-ready implementation with tests
- Error handling, logging/observability, and configuration in place
- Build/lint/test passing and main flows smoke-tested
- Deploy/runbook notes and handoff flags for `/sweeper` and `/maintainer`

## Example

**Invocation:** `/builder the incident-context bot prototype that returned hardcoded recent changes`

**Response (abridged):**

> **Intake:** Spike returns a hardcoded summary. Gaps: real data source, auth,
> rate limits, errors when the data API is down, no tests.
>
> **Production bar:** unit + integration tests, graceful degradation when the
> changes API fails, structured logs, config for API endpoints/timeouts.
>
> **Plan:** `ChangesClient` (with retry/timeout) → `SummaryBuilder` → command
> handler. Postgres-free; reads from existing deploy + git APIs.
>
> **Built:** implemented all three modules with tests; added timeout + fallback
> ("couldn't reach change log, here's the last cached summary"); structured logging.
>
> **Verified:** `npm test` green (24 tests); smoke-tested live + API-down paths.
>
> **Handoff:** deploy via existing pipeline; `/maintainer` should add a CVE scan on
> the new HTTP dependency.
