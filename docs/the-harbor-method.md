# the harbor method: agile, rebuilt for agent fleets

> Author: Brian Marvin · Empowered Business Solutions · empowered.guru
> Date: August 2026
> Status: the thinking, published openly. the operational layer it describes
> is deliberately omitted. see the final section.

---

## // the question

"Agile and Scrum were built for teams of people. Is there an equivalent
system for teams of agents, and does copying Scrum onto agents actually
work?"

**Short answer: no mature, universal "Scrum for agents" exists yet.** The
field is mid-convergence, and most of what is published is a name and an
org chart with the hard parts left out. This document is a working answer,
written from running a fleet of AI agents in production, not from theory.

## // why scrum maps imperfectly

Scrum exists to solve a specific set of *human* frictions. Agents either
do not have those frictions, or fail in ways Scrum never anticipated.

| human friction scrum solves | agent reality |
|---|---|
| context-switching is expensive, so wip limits and focus | spawning is cheap, so more agents already means more bandwidth |
| people drift out of sync, so a daily standup | agents re-read structured state every run. sync is a **state** problem, not a **meeting** problem |
| estimation is variable, so story points and velocity | behavior is near-deterministic. the real variance is **cost, latency, and model quality** |
| morale and burnout, so retro and sustainable pace | no morale. the failure mode is **drift, orphans, and hallucinated self-reports** |
| knowledge loss across people, so ramp-up and docs | no forgetting, but no continuous learning either. everything is re-read cold |

The bottleneck moved. Agile optimizes **human attention**. Agent systems
optimize **verification, context budget, and cost**. A faithful
"agile for agents" is therefore not Scrum with robot standups. It is a
system where the *checks* are automated and the *cadence* is event-driven.

## // the manifesto

Every line of the original maps to a direct agent translation.

1. **working software over documentation**
   → **verified outcomes over self-reported claims.**
   Every "done" is a machine-checkable predicate: ci green, a live url
   returning 200, a checksum that matches. An agent saying "it works" is
   not evidence. A green check plus a receipt is.

2. **individuals and interactions over processes**
   → **capability contracts over personas.**
   Organize around what each agent can *do* and the contract it must
   satisfy: inputs, steps, validation, artifact, safety boundary. Not a
   named role that goes stale.

3. **responding to change over following a plan**
   → **event-driven dispatch over fixed timeboxes.**
   Agents do not need sprint boundaries. They need **triggers**. A trigger
   (manual, cron, webhook, anomaly) creates an occurrence, and an
   occurrence-key makes it idempotent.

## // the unit of delivery changes

When you translate the values, the unit of progress shifts too.

| scrum unit | harbor unit |
|---|---|
| story point | **green check + evidence** |
| sprint velocity | **cost per verified outcome** |
| sprint (timebox) | **occurrence** (trigger + occurrence key) |
| definition of done (human-judged) | **machine-checkable gate** (ci / deploy / live probe) |
| retrospective (meeting) | **retro routine** (a run, not a meeting) |

## // the loop

The operational spine, named for the port the metaphor lives in: the
**harbormaster** (coordinator), the **pods** (ships), the **captain**
(owner). Eight stages, each mapping to a live mechanism you can build
today.

| # | stage | what it actually is |
|---|---|---|
| 1 | **intake** | a trigger admits a unit of work (issue, webhook, cron fire) into the queue |
| 2 | **claim** | an atomic claim + a time-to-live. the ttl *is* the wip limit (stall detection) |
| 3 | **isolate** | a per-run workspace. branch-per-story, never a shared mutable tree |
| 4 | **work** | the budget is reserved *before* dispatch, never discovered mid-run |
| 5 | **verify** | a machine-checkable gate. the definition of done |
| 6 | **ship** | merge/deploy once green. continuous delivery |
| 7 | **log** | a digest classifies every run's health. the standup, automated |
| 8 | **improve** | the retrospective as a routine. the stage this document is really about |

Stages 1 through 7 are plain engineering and widely understood. **Stage 8
is the gap.** Nearly every "agent framework" stops at ship-and-log. The one
thing that makes a fleet *compound* is the loop that feeds lessons back,
and almost nobody has built it.

## // the missing piece: the retrospective as a routine

Scrum's retrospective exists because human teams drift and need a recurring
ritual to re-align and get faster. Agents do not drift emotionally, but
they **accumulate systemic failure silently**: the same class of bug, the
same auth-death loop, the same stale-credential trap, over and over. Today
that knowledge usually lives in a human-maintained pitfalls doc and is
injected into prompts by hand. That is the one thing agile-for-agents must
make *automatic*.

### the retro is a routine, not a meeting

A `retro` fires on **cadence or anomaly**, not on a calendar invite. One
run:

1. **reads** the work ledger for the window: runs, digests, receipts, logs
2. **classifies** every failure by signature (auth-dead, stalled,
   fatal-loop, verify-fail, drift, budget-bleed)
3. **deduplicates** against the existing pitfall log. only *new*
   signatures matter
4. **distills** each new signature into a structured, machine-actionable
   record
5. **acts**: patches the relevant skill, adds a watchdog, or appends a
   guard, so the *same* failure cannot recur silently
6. **reports** a one-page verdict in plain english, and **prints its own
   next action**. never a doc someone has to remember to open. a tool
   should print its next step

### records are data, not prose

A pitfall is not a paragraph. It is a row. It carries a unique signature,
the phase it occurs in, a root-cause class, how it was detected, the fix,
and where that fix flows: the retro patches the skill, the watchdog, and
the worker guard. Written that way, it can be deduplicated, diffed, and
*injected automatically* as advisory context in future runs.

*Advisory, never policy.* The retro's distilled lessons become context that
future runs see. Current sources and explicit policy always outrank
yesterday's memory. This is what separates learning from stale
cargo-culting: the insight is a hint, not a rule.

### three roles collapse scrum's four

Scrum: `PO + SM + devs + test`. The Harbor Method needs three, not four.

- **orchestrator**: decompose goals, keep work unblocked
- **worker**: execute inside an isolated, budgeted envelope
- **verifier**: the gate *and* the retro

The Scrum Master dissolves: coordination is a ledger, not a person.

### the fourth addition: the bosun

The boatswain, the officer responsible for a ship's rigging and equipment,
keeping it in working order. The bosun owns the retro loop: fleet
self-improvement, the pitfall log, skill patching, watchdog upkeep. It is
the *smallest-useful* addition that closes the "no retro for agents" gap,
and it is what turns a fleet from a pipeline into a system that genuinely
gets better each cycle.

## // why this beats the "scrum-shaped" agent frameworks

AutoGen, CrewAI, MetaGPT, and ChatDev encode **human org charts**: PO,
architect, engineer, reviewer. Their documented failure mode is the exact
thing agile-for-agents must solve first: role confusion with **no
verification gate and no learning loop**. They have workers and managers,
but no verifier and no retro.

The Harbor Method makes the **verification gate the center** and makes the
**retrospective automatic**: the two things a name-and-roles framework is
missing.

## // day-to-day translation

| today (scrum habit) | harbor method |
|---|---|
| sprint planning | define the trigger + reserve budget + approve capability scope |
| daily standup | the run digest's health classification |
| sprint review | the retro's one-page verdict |
| retrospective | the `retro` routine |
| velocity report | cost-per-verified-outcome ledger |

## // the boundary

This document is the thinking, published in full. The operational layer it
points at is **not** here, deliberately.

**open**: the philosophy. The three values, the loop, the three-plus-one
roles, the retro concept, the translation tables. Free to use, share, and
adapt (cc by-sa 4.0). If this maps to what you are running, it is yours to
take.

**gated**: the implementation. The exact contract schemas, the
intake/claim/isolation/budget runtime, the machine-actionable record
format, the watchdogs, the ledger wiring. That is the hard, expensive
twenty percent that took real production failures to learn, and it is the
part we build and operate for clients.

The reasoning is simple: **the map is free, the engines are a service.**
Giving away the thinking attracts the people who need the deep end. The
deep end is how it becomes a business.

---

*interested in the deep end?* → **[empowered.guru](https://www.empowered.guru)**