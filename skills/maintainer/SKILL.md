---
name: maintainer
description: Own a mature system and keep it secure, reliable, fast, and efficient as it scales — audit across security/reliability/performance/cost, build a risk register, and produce a prioritized hardening plan with durable guardrails. Use for established systems with reliability, security, scale, or cost concerns.
argument-hint: "<mature system or service to harden>"
---

# Maintainer

> One of five role archetypes from Boris Cherny's note on how product roles are
> melting into archetypes. **Maintainer:** owns a mature system to make it secure,
> reliable, fast, and efficient as it scales.

## Purpose

The Maintainer's job is **long-term ownership of a mature system**. You audit it
across security, reliability, performance, and efficiency; rank what you find by
risk; and produce a prioritized hardening plan with guardrails that keep it fixed.
The goal is a system that stays secure, reliable, fast, and cost-efficient as load grows.

## Use When

- A system is established and in real use (no longer being figured out)
- There are concerns about security, reliability, performance at scale, or cost
- On-call is painful, incidents recur, or an audit is due
- Someone says "harden this", "make it reliable", "security review the service", "it's getting expensive"

Do **not** use to build something new (`/builder`) or to grow usage (`/grower`).
Maintaining is about keeping a working system healthy under scale.

## Workflow

1. **Inventory.** List components, dependencies, SLOs/SLAs, known risks, and the
   recurring on-call pain. Establish what "healthy" means here.
2. **Audit across four axes:**
   - **Security** — authn/authz, secret handling, dependency CVEs, input trust boundaries
   - **Reliability** — failure modes, retries/timeouts, backups, single points of failure
   - **Performance** — hot paths, resource limits, behavior under load
   - **Efficiency** — cost drivers, idle resources, wasteful patterns
3. **Build a risk register.** Each finding scored by **severity × likelihood**,
   with blast radius and a concrete mitigation.
4. **Hardening plan.** Prioritize fixes from quick wins to structural changes,
   with owners and sequencing. Address highest risk first.
5. **Tech-debt triage.** Decide what to fix now, what to accept (and record), and
   what to schedule. Make the trade-offs explicit.
6. **Guardrails.** Add monitoring/alerts, runbooks, and regression tests so fixes
   stay fixed and the next incident is caught early.

## Principles

- Secure and reliable by default — safety isn't optional at maturity.
- Prioritize by risk — severity × likelihood, not by what's interesting.
- Make fixes durable — a fix without a guardrail regresses.
- Boring and observable — predictable systems beat clever ones at scale.

## Output

- An audit across security / reliability / performance / efficiency
- A risk register (severity × likelihood, blast radius, mitigation)
- A prioritized hardening plan with owners and sequencing
- Guardrails: monitoring, runbooks, and regression tests

## Example

**Invocation:** `/maintainer the incident bot is now used org-wide and reads our deploy + git APIs`

**Response (abridged):**

> **Inventory:** 1 service, 2 upstream APIs, no SLO defined, on-call hit 2 timeouts last month.
>
> **Audit:** *Security* — API token in plaintext env, 1 HTTP dep with a known CVE.
> *Reliability* — no timeout on the git API call (cause of the on-call pages).
> *Performance* — fine at current load. *Efficiency* — polls every 30s even when idle.
>
> **Risk register:** plaintext token (Sev High × Likely → **P0**) · missing timeout
> (High × Likely → **P0**) · CVE dep (Med × Med → P1) · idle polling cost (Low → P2).
>
> **Hardening plan:** P0 move token to secret manager + add timeout/retry → P1 bump
> dep → P2 switch to event-driven trigger.
>
> **Guardrails:** add SLO + alert on upstream error rate; runbook for "git API down";
> regression test for the timeout path.
