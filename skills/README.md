# the skills

>_ the adoptable layer of the harbor method

Five role contracts, written so you can copy them into your own fleet
today. Each follows the same anatomy:

```
// purpose      what the role exists to do
// when to use  the trigger that summons it
// the loop     the steps, in order
// failure modes  how this role fails, and how to catch it
// verification   the check that proves it worked
// output         the artifact it must produce
```

These are the *map*, not the engines. The values and the roles are open.
The runtime that wires them together (contract schemas, budget
reservation, ledger, watchdogs) is what empowered.guru builds and operates.

## the roster

| skill | role | owns |
|---|---|---|
| [`orchestrator`](orchestrator/SKILL.md) | decompose goals, keep work unblocked | intake, claim, ship |
| [`worker`](worker/SKILL.md) | execute inside a budgeted envelope | isolate, work |
| [`verifier`](verifier/SKILL.md) | the gate and the retro | verify, log |
| [`bosun`](bosun/SKILL.md) | the self-improvement loop | improve |
| [`retro`](retro/SKILL.md) | the retrospective, as a routine | improve |

## how they fit

```
             ┌─────────────┐
 trigger ──► │ orchestrator│──► unit of work ──┐
             └─────────────┘                   ▼
                                       ┌─────────────┐
                                       │   worker    │
                                       └─────────────┘
                                              │ artifact
                                              ▼
                                       ┌─────────────┐
                                       │  verifier   │── pass ──► ship
                                       └─────────────┘
                                              │ fail
                                              ▼
                                       ┌─────────────┐
                                       │  bosun      │──► retro ──► patch
                                       └─────────────┘
```

The verifier fails a thing. The bosun turns that failure into a lesson. The
retro patches the skill so the same failure cannot recur silently. That
loop, from `verify` back to `work`, is the entire point.

---

*The full philosophy:* [`docs/the-harbor-method.md`](../docs/the-harbor-method.md)