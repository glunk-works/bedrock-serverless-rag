# Next steps — dev-workflow cursor

Thin, live cursor for whoever picks up this repo next. Points into the deep record
(`docs/hardening_roadmap.md`, the sprint plans, the issues) — it does not copy them.
Regenerate this at the end of every working session.

## Now

**`S2`, `implementing`.** `SD` is parked (`.ai/parked/SD-*`) — this is a deliberate swap, not a
scope change: `SD` Task 4 is docs-only and can wait, and `S2`'s remaining work is the more
urgent live thread.

## Just done

- **parked `SD`:** Canary exercise for way-of-working v0.8.0 park-sprint/unpark-sprint skills
  (`claude-workbench#36`).

## Next

**Model: `sonnet` / coder** (assigned for whenever the gate below clears — this action itself is
the human's, not a coding task).

1. **`S2` Task 4 step 3, sub-step 3.2** — merge to `main` queues a `tofu-apply` plan of
   `12 to add`; click the `production` Environment approval to let it build the lab under the
   new upstream role.
2. **`S2` Task 4 step 3, sub-step 3.3** — once built, dispatch `destroy-ai-lab` with the typed
   confirm phrase and watch it live (BR-D25), proving the destroy verbs too. This is the full
   BR-D20 cycle Task 4's own verify requires, run while the old `bootstrap/` role still exists
   as a fallback.

   Full spec: `grep -n '^### Task 4' -A 60 sprints/S2_identity_least_privilege/sprint_plan.md`.

Also outstanding on the v0.8.0 canary (`claude-workbench#36`): run `/way-of-working:architect-review`
against a real PR here to confirm it correctly hits its own step-1 no-gate stop
(`review.ci_gate` is `null` — BR-D14, which itself says to reconsider only once S2 has landed,
which is exactly why this repo isn't wired up for a full posting run today). Then
`/way-of-working:unpark-sprint SD` to complete the round-trip and close out the canary.

## Open gates and blockers

**HITL Gate: OPEN.** Sub-step 3.2 needs the user's `production` Environment approval click on
the queued `tofu-apply`; sub-step 3.3 needs the user to dispatch `destroy-ai-lab` with the typed
confirm phrase and watch it live per BR-D25. **Neither may be started unattended** —
`/way-of-working:resume` must wait on this cursor until the human has acted.

## Pointers

- `docs/hardening_roadmap.md` — reference of record and threat model. `BR-D14` is the entry
  that ties `architect-review` readiness to `S2` landing.
- `sprints/S2_identity_least_privilege/sprint_plan.md` — Task 4 step 3: 3.1 satisfied,
  3.2/3.3 pending human action.
- `.ai/parked/` — holds `SD`'s snapshot (`parked_at` 2026-09-23). Restore with
  `/way-of-working:unpark-sprint SD`.
