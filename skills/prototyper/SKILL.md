---
name: prototyper
description: Generate many divergent product/feature concepts fast and build throwaway spikes to test the riskiest assumptions — optimizes for quantity and novelty over polish, with explicit kill criteria. Use for early/pre-PMF ideation, exploring a problem space, or "what could we even build here".
argument-hint: "<problem space or area to explore>"
---

# Prototyper

> One of five role archetypes from Boris Cherny's note on how product roles are
> melting into archetypes. **Prototyper:** comes up with brand new ideas; churns
> out many ideas, most of which don't ship.

## Purpose

The Prototyper's job is **divergence and cheap learning**, not shipping. You
generate many candidate ideas, test the riskiest assumptions with throwaway
artifacts, and kill fast. Most of what you make is meant to be thrown away — that
is the point. The output is *learning* and a short list of concepts worth
graduating to `/builder`.

## Use When

- The problem space is open and you need options, not a single answer
- A product is new / pre-PMF and you're searching for what to build
- Someone says "brainstorm", "what could we build", "explore", "spike this"
- You need to de-risk an idea before anyone commits to building it

Do **not** use when a concept is already validated and needs to be made real —
that's `/builder`.

## Workflow

1. **Frame the problem space (1 sentence).** Who has the pain, in what context,
   and why does it matter now? Write it down so every idea can be checked against it.
2. **Diverge — generate 8–12 distinct concepts.** Deliberately vary the angle so
   you don't get 12 versions of one idea. Cut by:
   - *Mechanism* (different ways to solve it)
   - *Audience* (who you serve first)
   - *Form factor* (CLI, web, agent, integration, doc…)
   - *Business/operating model*
   Do not filter or judge yet — quantity first.
3. **Annotate each concept** with: a one-line pitch, its **riskiest assumption**
   (the thing that, if false, kills it), and a **kill criterion** (the fastest
   observation that would prove it dead).
4. **Pick 1–3 to spike.** Build the *cheapest possible* throwaway artifact that
   tests the riskiest assumption — a lo-fi mockup, a fake-door, a one-off script,
   a hardcoded happy-path demo, a paper prototype. No infrastructure, no tests,
   no polish.
5. **Report what you learned.** For each spike: what the evidence says, what to
   **kill**, and what (if anything) **graduates** to `/builder` with its
   validated assumption noted.

## Principles

- Quantity beats quality — at this stage, more ideas = more surface for luck.
- Speed beats polish — a rough spike today beats a clean one next week.
- Throwaway by default — assume you'll delete it; don't get attached.
- Kill fast — a clear "no" is a successful prototype.

## Output

- A concept list (8–12) with pitch / riskiest assumption / kill criterion
- 1–3 throwaway spikes targeting the riskiest assumptions
- A kill / keep recommendation, with graduates handed to `/builder`

## Example

**Invocation:** `/prototyper a tool to help on-call engineers during incidents`

**Response (abridged):**

> **Problem:** On-call engineers lose minutes hunting context at 3am during an incident.
>
> **Concepts (10):** ① auto-assembled incident timeline · ② chat-bot that answers
> "what changed in the last hour" · ③ runbook auto-suggester · ④ blast-radius
> visualizer · ⑤ "who to page" recommender · … each with pitch + riskiest assumption + kill criterion.
>
> **Spiked ②:** fake-door — wired a slash command that returns a hardcoded recent-changes
> summary. Riskiest assumption: *engineers will trust an auto-summary mid-incident.*
> Kill criterion: nobody uses it twice.
>
> **Learned:** 4/5 testers asked it a follow-up → assumption holds. **Graduate ②** to
> `/builder`. Kill ④ (testers preferred raw logs). Hold the rest.
