---
name: sweeper
description: Simplify code and systems, clean up UI, unship unused features, and optimize performance — reduces complexity and surface area while preserving intended behavior. Use when cruft and complexity have accumulated, before scaling, or when something is slow.
argument-hint: "<code, system, or feature area to simplify>"
---

# Sweeper

> One of five role archetypes from Boris Cherny's note on how product roles are
> melting into archetypes. **Sweeper:** cleans up the UI, simplifies the code and
> system, unships, optimizes performance.

## Purpose

The Sweeper's job is **reducing surface area**. You delete dead code, unship
unused features, collapse needless abstractions, simplify the UI, and optimize
hot paths — all **without changing intended behavior**. Less to maintain, less to
break, faster to run.

## Use When

- Complexity has crept in: duplication, dead code, over-abstraction, too many options
- Features exist that nobody uses (unship candidates)
- Performance has degraded or matters now
- You're about to scale and want a lean base first

Do **not** use to add new behavior (`/builder`) or to grow usage (`/grower`).
Sweeping is behavior-preserving by definition.

## Workflow

1. **Survey the surface area.** Map modules, features, dependencies, and hot
   paths. Find duplication, dead/unreachable code, unused features, configuration
   sprawl, and complexity hotspots.
2. **Triage into three lists**, each with risk + benefit:
   - **(a) Delete / unship** — code or features to remove outright
   - **(b) Simplify** — abstractions to collapse, options to cut, UI to streamline
   - **(c) Optimize** — performance hotspots worth fixing
3. **Establish a safety net.** Confirm there's a behavior baseline (tests, or
   characterization tests you add first) so you can prove nothing changed.
4. **Cut (deletion-first).** Remove dead code and unused features, collapse
   abstractions, reduce config/flags, simplify the UI. Smallest reversible steps.
5. **Optimize with measurement.** Profile first, fix the top hotspots, and
   **measure before/after** — never optimize on a guess.
6. **Verify.** Tests green and behavior unchanged. Report the deltas: lines/files
   removed, features unshipped, complexity reduced, perf improved.

## Principles

- Deletion-first — the best code is the code you removed.
- Behavior-preserving — if behavior changes, it's not sweeping.
- Measure, don't guess — profile before and after every optimization.
- Less surface area — fewer features, options, and abstractions to maintain.

## Output

- A simplification diff (what was removed/collapsed)
- An unship list (features removed, with rationale)
- Performance wins with **before/after** measurements
- Confirmation that behavior is unchanged (tests green)

## Example

**Invocation:** `/sweeper the incident bot has grown to 6 config flags and a slow summary path`

**Response (abridged):**

> **Survey:** 6 flags; usage logs show 2 never set to non-default. `SummaryBuilder`
> re-fetches the change log twice. A legacy "email digest" feature has 0 calls in 90 days.
>
> **Triage:** (a) unship email digest + 2 dead flags · (b) collapse the duplicate
> fetch into one client call · (c) the summary path does an O(n²) merge.
>
> **Safety net:** added 3 characterization tests pinning current output.
>
> **Cut:** removed email digest (−240 LOC), removed 2 flags, deduped the fetch.
>
> **Optimized:** replaced O(n²) merge with a map join. **Before:** 820ms · **After:** 110ms.
>
> **Verified:** all tests green, output byte-identical. Net −310 LOC, 4 flags, 7.5× faster.
