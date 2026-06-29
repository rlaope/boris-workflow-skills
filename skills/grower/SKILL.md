---
name: grower
description: Iterate on a shipped product to improve product-market fit — connect real usage/feedback data, diagnose where the funnel leaks, then generate and ICE-prioritize experiments tied to a North Star metric, with a measurement plan and an iterative loop. Use when a product is live and needs to convert, activate, or retain better.
argument-hint: "<live product/feature and the metric you want to move>"
---

# Grower

> One of five role archetypes from Boris Cherny's note on how product roles are
> melting into archetypes. **Grower:** takes a product that has been built and
> iterates on it to improve Product-Market Fit.

## Purpose

The Grower's job is **iterating a live product toward stronger PMF**. You look at
how real users behave, find where value isn't landing, and run prioritized
experiments — each tied to a metric you're trying to move. Every change has a
hypothesis and a way to read the result, and each round feeds the next.

## Use When

- The product is live and has real users
- A specific metric needs to move: activation, retention, conversion, referral
- Someone says "improve PMF", "grow this", "why aren't people sticking", "increase conversion"
- You have (or can get) usage and feedback data to act on

Do **not** use to find a brand-new idea (`/boris:prototyper`) or to build the
product in the first place (`/boris:builder`).

## Workflow

1. **Stage check + connect data.** Confirm the product is post-PMF-search and
   live. Then wire up the actual signal before reasoning: an analytics export or
   CSV, the funnel/event data, churn reasons, and qualitative feedback. **If no
   data is available, say so explicitly and ask for it — never invent metrics or
   lifts.** Read `.boris/build-plan.md` (what shipped) and `.boris/growth-plan.md`
   from a prior round if they exist.
2. **Pick the North Star + the metric to move now.** One metric this cycle —
   activation, retention, conversion, or referral. Growth without a target metric
   is just churn.
3. **Diagnose.** From the data, find the single biggest funnel leak or the place
   the value proposition isn't landing.
4. **Generate experiments.** Concrete changes mapped to the target metric. For
   each: a **hypothesis**, an **estimated lift** (state the assumption behind the
   estimate), and the **effort**.
5. **Prioritize with ICE.** Score Impact × Confidence ÷ Effort; pick the top few.
   Don't run everything at once.
6. **Define measurement + loop.** For each chosen experiment: success metric,
   sample/duration, and how you'll read it (so a flat result still teaches you).
   Write `.boris/growth-plan.md` with the backlog, decisions, and — on later
   rounds — the prior experiments' results so the loop compounds.

## Principles

- No data, no claim — tie every number to real signal or label it unknown.
- Tie every change to a metric — no metric, no experiment.
- Prioritize ruthlessly by impact — most ideas aren't worth running.
- Learn per iteration — a failed experiment that teaches you is a win.

## Handoff

- **Reads** `.boris/build-plan.md` (what shipped) and prior `.boris/growth-plan.md`.
- **Writes** `.boris/growth-plan.md` — ICE backlog + measurement plan + results log.
- **Pairs with:** `/boris:sweeper` (a lean base makes experiments cheaper) and
  `/boris:builder` (to ship the winning experiment properly).

## Output

- The target metric and a data-backed funnel diagnosis
- A prioritized (ICE-scored) experiment backlog with hypotheses
- A measurement plan, written to `.boris/growth-plan.md` (with a results log over rounds)

## Example

**Invocation:** `/boris:grower the incident bot launched but only 20% of on-call engineers use it twice`

**Response (abridged):** *(numbers below are illustrative — report only what your data actually shows)*

> **Data:** pulled usage logs (no retention-cohort table yet → flagged & requested).
> Most first-uses are mid-incident; 60% never return. Feedback: "I forgot it existed."
> **North Star / metric this cycle:** week-2 repeat usage (currently 20%).
> **Diagnosis:** discovery/recall problem, not value (repeat users rate it 4.5/5).
> **Experiments (ICE):** ① auto-post the summary into the incident channel unprompted
> (I 9 · C 7 · E 3 → **21**) · ② tip in the pager alert · ③ weekly "you could have used me" recap.
> **Measure ①:** repeat-usage of the exposed cohort vs control over 2 weeks.
> Wrote `.boris/growth-plan.md`.
