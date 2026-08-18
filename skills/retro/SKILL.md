---
name: retro
role: the retrospective, as a routine
owns: improve
---

# retro

>_ the retrospective is a run, not a meeting.

## // purpose

Do what Scrum's retro did, automatically: read the ledger of everything
that ran, classify every failure by signature, dedupe against the pitfall
log, distill new lessons, and fire the fixes. It runs on cadence or
anomaly, not on a calendar invite.

## // when to use

- on a schedule (the cadence: end of a cycle)
- on an anomaly (a fatal-loop, a stall, a budget bleed detected)

## // the loop

1. **read** the ledger for the window: runs, digests, receipts, logs
2. **classify** every failure by signature (auth-dead, stalled,
   fatal-loop, verify-fail, drift, budget-bleed)
3. **dedupe** against the pitfall log. only *new* signatures matter
4. **distill** each new signature into a structured record
5. **act**: patch the skill, add a watchdog, append a guard. the same
   failure cannot recur silently
6. **report**: a one-page verdict in plain english, plus **its own next
   action**. never hand back a doc someone has to remember to open

## // failure modes

- **reading nothing**: running the retro against an empty or stale ledger.
  the ledger is the input, and garbage in means garbage out
- **re-reporting old failures**: skipping dedupe means the same lessons
  get "discovered" every cycle
- **reporting without action**: a verdict is only useful if the next
  action is attached and someone (the bosun) owns it
- **mutating live state**: a read-only retro. the retro must never write
  to a running system while it is still running

## // verification

- no stale signature was re-reported
- every new signature produced a structured record and a fix
- the output ends with a concrete next action, not just a summary

## // output

```text
// retro verdict · cycle 34
  3 runs  2 failed  1 new signature

  new: auth-dead: provider-401-after-key-rotation
    phase: work · class: stale-credential
    fix: rotate + re-seed + login probe
    action: bosun patches auth-watchdog

  next action: bosun apply patch -> then widen retro to anomaly triggers
```

*shape, not a runtime contract.*