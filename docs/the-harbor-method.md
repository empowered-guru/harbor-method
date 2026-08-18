# The Harbor Method — Agile, Rebuilt for Agent Fleets

> Author: Brian Marvin · Empowered Business Solutions · https://empowered.guru
> Date: August 2026
> Status: the thinking, published openly. The operational layer it describes is
> deliberately omitted — see the final section.

---

## 1. The question

"Agile and Scrum were built for teams of people. Is there an equivalent system for
teams of agents — and does copying Scrum onto agents actually work?"

**Short answer: no mature, universal "Scrum for agents" exists yet.** The field is
mid-convergence, and most of what's published is a name-and-an-org-chart with the hard
parts left out. This document is a working answer, written from running a fleet of AI
agents in production — not from theory.

---

## 2. Why Scrum maps imperfectly

Scrum exists to solve a specific set of *human* frictions. Agents either don't have
those frictions, or fail in ways Scrum never anticipated.

| Human friction Scrum solves | Agent reality |
|---|---|
| Context-switching is expensive → WIP limits, focus | Spawning is cheap → "more bandwidth by adding agents" already rejects the premise |
| People drift out of sync → daily standup | Agents re-read structured state every run; sync is a **state** problem, not a **meeting** problem |
| Estimation is variable → story points, velocity | Behavior is near-deterministic; the real variance is **cost, latency, and model quality** |
| Morale and burnout → retro, sustainable pace | No morale; the failure mode is **drift, orphans, and hallucinated self-reports** |
| Knowledge loss across people → ramp-up, docs | No forgetting, but no continuous learning either — everything is re-read cold |

The bottleneck moved. Agile optimizes **human attention**. Agent systems optimize
**verification, context-window budget, and cost**. A faithful "agile for agents"
therefore isn't Scrum with robot standups — it's a system where the *checks* are
automated and the *cadence* is event-driven.

---

## 3. The Agent Agile Manifesto

Every line of the original map has a direct agent translation:

1. **Working software over documentation**
   → **Verified outcomes over self-reported claims.**
   Every "done" is a machine-checkable predicate: CI green, live URL returning 200, a
   checksum that matches. An agent saying "it works" is not evidence; a green check
   plus a receipt is.

2. **Individuals and interactions over processes**
   → **Capability contracts over personas.**
   Organize around what each agent can *do* and the contract it must satisfy — inputs,
   steps, validation, artifact, safety boundary — not a named role that goes stale.

3. **Responding to change over following a plan**
   → **Event-driven dispatch over fixed timeboxes.**
   Agents don't need sprint boundaries; they need **triggers**. A trigger (manual, cron,
   webhook, anomaly) creates an occurrence, and an occurrence-key makes it idempotent.

---

## 4. The unit of delivery changes

When you translate the values, the unit of progress shifts too.

| Scrum unit | Harbor unit |
|---|---|
| Story point | **Green check + evidence** |
| Sprint velocity | **Cost per verified outcome** |
| Sprint (timebox) | **Occurrence** (trigger + occurrence key) |
| Definition of done (human-judged) | **Machine-checkable gate** (CI / deploy / live probe) |
| Retrospective (meeting) | **Retro routine** (a run, not a meeting — §6) |

---

## 5. The Harbor loop

The operational spine, named for the port the metaphor lives in: the **Harbormaster**
(coordinator), the **pods** (ships), the **Captain** (owner). Eight stages, each mapping
to a live mechanism you can build today:

| # | Stage | What it actually is |
|---|---|---|
| 1 | **Intake** | A trigger admits a unit of work (issue, webhook, cron fire) into the queue |
| 2 | **Claim** | An atomic claim + a time-to-live — the TTL *is* the WIP limit (stall detection) |
| 3 | **Isolate** | A per-run workspace; branch-per-story, never a shared mutable tree |
| 4 | **Work** | The budget is reserved *before* dispatch, never discovered mid-run |
| 5 | **Verify** | A machine-checkable gate — the definition of done |
| 6 | **Ship** | Merge/deploy once green; continuous delivery |
| 7 | **Log** | A digest classifies every run's health — the standup, automated |
| 8 | **Improve** | ★ the retrospective as a routine — the stage this document is really about |

Stages 1–7 are plain engineering and widely understood. **Stage 8 is the gap.** Nearly
every "agent framework" stops at ship-and-log. The one thing that makes a fleet
*compound* is the loop that feeds lessons back — and almost nobody has built it.

---

## 6. The missing piece: the retrospective as a routine

Scrum's retrospective exists because human teams drift and need a recurring ritual to
re-align and get faster. Agents don't drift emotionally — but they **accumulate systemic
failure silently**: the same class of bug, the same auth-death loop, the same
stale-credential trap, over and over. Today that knowledge usually lives in a
human-maintained "pitfalls doc" and is injected into prompts by hand. That is the one
thing agile-for-agents must make *automatic*.

### 6.1 The retro is a routine, not a meeting

A `retro` fires on **cadence or anomaly**, not on a calendar invite. One run:

1. **Reads** the work ledger for the window — runs, digests, receipts, logs.
2. **Classifies** every failure by signature (auth-dead, stalled, fatal-loop,
   verify-fail, drift, budget-bleed).
3. **Deduplicates** against the existing pitfall log — only *new* signatures matter.
4. **Distills** each new signature into a structured, machine-actionable record.
5. **Acts** — patches the relevant skill, adds a watchdog, or appends a guard — so the
   *same* failure cannot recur silently.
6. **Reports** a one-page verdict in plain English, and **prints its own next action.**
   Never a doc someone has to remember to open. A tool should print its next step.

### 6.2 Records are data, not prose

A pitfall is not a paragraph — it's a row. It carries a unique signature, the phase it
occurs in, a root-cause class, how it was detected, the fix, and where that fix flows
(retro patches the skill, the watchdog, and the worker guard). Written that way, it can
be deduplicated, diffed, and *injected automatically* as advisory context in future
runs.

*Advisory, never policy.* The retro's distilled lessons become context that future runs
see — current sources and explicit policy always outrank yesterday's memory. This is
what separates "learning" from "stale cargo-culting": the insight is a hint, not a rule.

### 6.3 Three roles collapse Scrum's four

Scrum: `PO + SM + devs + test`. The Harbor Method needs three, not four:

- **Orchestrator** — decomposes goals, keeps work unblocked.
- **Worker** — executes inside an isolated, budgeted envelope.
- **Verifier** — the gate **and** the retro.

The "Scrum Master" role dissolves: coordination is a ledger, not a person.

### 6.4 The fourth addition: the **Bosun**

The boatswain — the officer responsible for a ship's rigging and equipment, keeping it
in working order. The Bosun owns the retro loop: fleet self-improvement, the pitfall
log, skill patching, watchdog upkeep. It's the *smallest-useful* addition that closes
the "no retro for agents" gap — and it's what turns a fleet from a pipeline into a
system that genuinely gets better each cycle.

---

## 7. Why this beats the "Scrum-shaped" agent frameworks

AutoGen, CrewAI, MetaGPT, and ChatDev encode **human org charts** (PO, architect,
engineer, reviewer). Their documented failure mode is the exact thing agile-for-agents
must solve first: role confusion with **no verification gate and no learning loop.** They
have workers and managers, but no `Verifier` and no retro.

The Harbor Method makes the **verification gate the center** and makes the
**retrospective automatic** — the two things a name-and-roles framework is missing.

---

## 8. Day-to-day translation

| Today (Scrum habit) | Harbor Method |
|---|---|
| Sprint planning | Define the trigger + reserve budget + approve capability scope |
| Daily standup | The run digest's health classification |
| Sprint review | The retro's one-page verdict |
| Retrospective | The `retro` routine |
| Velocity report | Cost-per-verified-outcome ledger |

---

## 9. The boundary

This document is the thinking, published in full. The operational layer it points at is
**not** here — deliberately.

**Open:** the philosophy. The three values, the loop, the three-plus-one roles, the
retro concept, the translation tables. Free to use, share, and adapt (CC BY-SA 4.0).
If this maps to what you're running, it's yours to take.

**Gated:** the implementation. The exact contract schemas, the intake/claim/isolation/
budget-reservation runtime, the machine-actionable record format, the watchdogs, the
ledger wiring. That is the hard, expensive 20% — the part that took real production
failures to learn — and it's the part we build and operate for clients.

The reasoning is simple: **the map is free, and the engines are a service.** Giving away
the thinking attracts the people who need the deep end. The deep end is how it becomes a
business.

---

*Interested in the deep end?* **[empowered.guru](https://www.empowered.guru)**