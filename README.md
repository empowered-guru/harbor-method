# The Harbor Method

**Agile, rebuilt for AI agent fleets.**

Scrum and Agile were designed for teams of *people*. They solve human problems —
expensive context-switching, drift, variable estimates, burnout. Agents don't have
those problems, and they fail in ways Agile never anticipated: drift, orphaned runs,
hallucinated self-reports.

The Harbor Method is a working answer to a simple question:

> *Is there an agile for agents — and does copying Scrum onto agents actually work?*

Short answer: no mature "Scrum for agents" exists yet. But the principles translate.
You don't get there by giving a robot a standup meeting. You get there by translating
what the Agile values were *for*, one layer down.

This repo is that translation.

---

## The Agent Agile Manifesto

Every line of the original maps to a direct agent translation:

| Original Agile value | Agent translation |
|---|---|
| Working software over documentation | **Verified outcomes over self-reported claims.** "It works" is not evidence — a green check plus a receipt is. |
| Individuals and interactions over processes | **Capability contracts over personas.** Organize around what each agent can *do* and the contract it must satisfy, not a named role that goes stale. |
| Responding to change over following a plan | **Event-driven dispatch over fixed timeboxes.** Agents don't need sprints — they need *triggers*. |

The bottleneck moved. Agile optimizes **human attention**; agent systems optimize
**verification, context-window budget, and cost**. Build for the thing that actually
breaks.

---

## The loop

Eight stages. The first seven are mundane engineering — the eighth is the part nobody
has shipped yet.

1. **Intake** — a trigger admits a unit of work (an issue, a webhook, a cron fire).
2. **Claim** — an atomic claim + a time-to-live. The TTL *is* the WIP limit.
3. **Isolate** — work happens in an isolated envelope (branch-per-story, not one shared tree).
4. **Work** — the budget is reserved *before* dispatch, never discovered mid-run.
5. **Verify** — a machine-checkable gate *is* the definition of done.
6. **Ship** — merge/deploy once green. Continuous delivery.
7. **Log** — a digest classifies every run's health. This is the standup, automated.
8. **Improve** — ★ *the retrospective, as a routine.* The new piece.

## Three roles (not four)

Scrum's `PO + SM + devs + test` collapses to three. The "Scrum Master" is absorbed —
coordination is a ledger, not a person.

- **Orchestrator** — decomposes goals, keeps work unblocked.
- **Worker** — executes inside an isolated, budgeted envelope.
- **Verifier** — the gate **and** the retro.

Plus one addition that closes the gap: the **Bosun** — the officer who keeps a ship's
rigging in working order. It owns the loop that makes the fleet get better each cycle.

---

## Read the thinking

The full essay — why Scrum doesn't map, the unit-of-delivery shift, the retro-as-a-
routine, and why this beats the "Scrum-shaped" agent frameworks — is in
[docs/the-harbor-method.md](docs/the-harbor-method.md).

If you'd rather the short version, it's a blog post:
**[Agile is broken for AI agents — here's what I use instead](https://www.empowered.guru/blog/agile-is-broken-for-ai-agents-heres-what-i-use-instead)**

---

## What's open, and what isn't

This repo is **deliberately** a philosophy-and-framework, not an implementation.

**Open (and free):** the thinking. The values, the loop, the roles, the retro concept,
the translation tables. Use them, adapt them, ship with them. That's the point —
generosity is how you find the people who need the deeper thing.

**Gated (what we're paid to build and run):** the operational layer. The exact contract
schemas, the intake/claim/isolation/budget-reservation runtime, the machine-actionable
pitfall-record format, the watchdogs, the ledger wiring. That's the "how," and it's the
part worth paying for.

The map is free. The engines are a service.

→ **[empowered.guru](https://www.empowered.guru)** — we operate agent fleets this way for
clients. If this resonates, that's where the deep end lives.

---

## Author

**Brian Marvin** — founder, [Empowered Business Solutions](https://www.empowered.guru).
Written from running a fleet of AI agents in production, not from theory.

## License

[CC BY-SA 4.0](LICENSE) — the writing is free to share and adapt with attribution.
The operational implementation it describes is not included here.

---

*This repository is a public continuation of the thinking first published as
["Agile is broken for AI agents — here's what I use instead."](https://www.empowered.guru/blog/agile-is-broken-for-ai-agents-heres-what-i-use-instead)*