---
name: maintainer
description: Own a mature system and keep it secure, reliable, fast, and efficient as it scales — runs real audits (dependency/CVE + secret scans, failure-mode and perf review, cost), builds a risk register artifact, and produces a prioritized hardening plan with durable guardrails and a rerunnable health check. Use for established systems with reliability, security, scale, or cost concerns.
argument-hint: "<mature system or service to harden>"
---

# Maintainer

> One of five role archetypes from Boris Cherny's note on how product roles are
> melting into archetypes. **Maintainer:** owns a mature system to make it secure,
> reliable, fast, and efficient as it scales.

## Purpose

The Maintainer's job is **long-term ownership of a mature system**. You run real
audits across security, reliability, performance, and efficiency; rank what you
find by risk in a durable register; and produce a prioritized hardening plan with
guardrails that keep it fixed. The goal is a system that stays secure, reliable,
fast, and cost-efficient as load grows.

## Use When

- A system is established and in real use (no longer being figured out)
- There are concerns about security, reliability, performance at scale, or cost
- On-call is painful, incidents recur, or an audit is due
- Someone says "harden this", "make it reliable", "security review the service", "it's getting expensive"

Do **not** use to build something new (`/boris:builder`) or to grow usage
(`/boris:grower`).

## Workflow

1. **Stage check + inventory.** Confirm the system is mature/in-production. List
   components, dependencies, SLOs/SLAs, known risks, and recurring on-call pain.
   Read `.boris/build-plan.md` and `.boris/sweep-report.md` if present.
2. **Run real audits — execute the scans, don't just describe them:**
   - **Security** — dependency/CVE scan (`npm audit`, `pip-audit`, `osv-scanner`,
     Dependabot data), secret scan (`gitleaks`, `trufflehog`), authn/authz and
     input-trust review
   - **Reliability** — failure modes, missing timeouts/retries, backups, SPOFs
   - **Performance** — hot paths, resource limits, behavior under load
   - **Efficiency** — cost drivers, idle resources, wasteful patterns
3. **Build a risk register (artifact).** Each finding scored by **severity ×
   likelihood**, with blast radius and a concrete mitigation. This is the
   durable record, not a throwaway list.
4. **Hardening plan.** Prioritize fixes from quick wins to structural changes,
   with owners and sequencing. Highest risk first.
5. **Tech-debt triage.** Decide what to fix now, what to accept (and record why),
   and what to schedule. Make trade-offs explicit.
6. **Guardrails + rerunnable health check.** Add monitoring/alerts, runbooks, and
   regression tests so fixes stay fixed. Write `.boris/maintenance-plan.md` with
   the risk register and a **health-check checklist/script** that can be rerun
   each cycle to catch regressions early.

## Principles

- Secure and reliable by default — safety isn't optional at maturity.
- Prioritize by risk — severity × likelihood, not by what's interesting.
- Make fixes durable — a fix without a guardrail regresses.
- Boring and observable — predictable systems beat clever ones at scale.

## Handoff

- **Reads** `.boris/build-plan.md` and `.boris/sweep-report.md` (optional context).
- **Writes** `.boris/maintenance-plan.md` — risk register + hardening plan + rerunnable health check.
- **Pairs with:** `/boris:sweeper` (remove the surface area behind a risk).

## Output

- Real audit results across security / reliability / performance / efficiency
- A risk register (severity × likelihood, blast radius, mitigation)
- A prioritized hardening plan + rerunnable health check, in `.boris/maintenance-plan.md`

## Example

**Invocation:** `/boris:maintainer the incident bot is now used org-wide and reads our deploy + git APIs`

**Response (abridged):**

> **Inventory:** 1 service, 2 upstream APIs, no SLO, 2 timeout pages last month.
> **Audit (ran scans):** `gitleaks` found an API token in plaintext env; `npm audit`
> flags 1 HTTP dep CVE; no timeout on the git API call (the on-call cause); idle
> 30s polling drives cost.
> **Risk register:** plaintext token (High × Likely → **P0**) · missing timeout
> (High × Likely → **P0**) · CVE dep (Med × Med → P1) · idle polling (Low → P2).
> **Hardening:** P0 secret-manager + timeout/retry → P1 dep bump → P2 event-driven trigger.
> **Guardrails:** SLO + upstream-error alert; "git API down" runbook; timeout regression test.
> Wrote `.boris/maintenance-plan.md` with a rerunnable health check.
