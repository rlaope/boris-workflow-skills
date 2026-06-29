# boris-workflow-skills

As engineering, product, design, and data science melt into a new kind of role, the work itself still divides into five archetypes. Following Boris Cherny's reflection on the Claude Code team — the Prototyper who churns out brand-new ideas that mostly don't ship, the Builder who turns a prototype into production-grade product and infrastructure, the Sweeper who cleans up the UI and simplifies the system and unships and optimizes, the Grower who iterates on a shipped product to improve product-market fit, and the Maintainer who owns a mature system and keeps it secure, reliable, fast, and efficient as it scales — this project releases each archetype as an open-source Claude Code skill. These roles are not tied to job function; a designer, an engineer, a PM, or a data scientist can each work in any of them. The skills here let you step into one archetype at a time and do exactly that.

<p align="center">
  <img src="./boris-post.png" alt="Boris Cherny's original post" width="560">
</p>

<br>

## Install

```
/plugin marketplace add rlaope/boris-workflow-skills
/plugin install boris@boris-workflow-skills
```

<br>

## Guide

What's your inclination right now? Tag the skill that fits it and ask it to set your direction.

Not sure which mode you're in? Start from where the product is — a healthy team needs a different mix at each stage:

| Product stage | Lean on | Start with |
|---|---|---|
| New / pre-PMF | Prototyper · Builder · Sweeper (1+2+3) | `/boris:prototyper` |
| Growing / found PMF | Builder · Sweeper · Grower, some Maintainer (2+3+4 + some 5) | `/boris:grower` |
| Strong PMF | Sweeper · Grower · Maintainer, some Builder (3+4+5 + some 2) | `/boris:maintainer` |

<br>

## How they chain

Each skill reads and writes a small markdown handoff under `.boris/` so one run can feed the next — `prototype.md` → `build-plan.md` → `sweep-report.md` / `growth-plan.md` / `maintenance-plan.md`. Every read is optional, so each skill also works standalone. `.boris/` is local working state — keep it gitignored.

---

Archetypes by Boris Cherny ([@bcherny](https://x.com/bcherny)). Licensed under [MIT](./LICENSE).
