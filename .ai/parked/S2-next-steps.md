# Next steps — dev-workflow cursor

Thin, live cursor for whoever picks up this repo next. Points into the deep record
(`docs/hardening_roadmap.md`, the sprint plans, the issues) — it does not copy them.
Regenerate this at the end of every working session.

## Now

**`S2`, `blocked`.** `SD` is parked (`.ai/parked/SD-*`). `S2` Task 4 step 3's cycle ran for
real and hit a genuine upstream bug — see below — and is now blocked on a fix in
`glunk-works/global-bootstrap`, not on a human click.

## Just done

- **Ran `S2` Task 4 step 3's human-gated sub-steps.** Approved the `tofu-apply` that had been
  queued since PR #129's merge (commit `0094f29`) via the `production` Environment gate —
  deploy run `35855923494` built 8 of 12 resources, then failed:
  `AccessDeniedException` on `aoss:CreateAccessPolicy` for the data-access policy's
  `index/bedrock-rag-store/*` rule. Root cause: `global-bootstrap`'s
  `WriteCollectionPoliciesForOurCollection` statement conditions on `aoss:collection`, and AWS
  documents that key as not covering index-level rules — the index case was never actually
  exercised before this apply. Filed as
  [glunk-works/global-bootstrap#17](https://github.com/glunk-works/global-bootstrap/issues/17)
  with root cause and a proposed companion statement, rather than fixed here (it's upstream
  IaC this repo doesn't own). Then dispatched `destroy-ai-lab` against the partial build —
  succeeded (run `35860478219`). **Nothing is live.**
- **Parked `SD`** ([PR #131](https://github.com/glunk-works/bedrock-serverless-rag/pull/131),
  merged): canary exercise for way-of-working v0.8.0's `park-sprint`/`unpark-sprint` skills
  (`claude-workbench#36`).
- **Confirmed `/way-of-working:architect-review`'s no-gate stop** against PR #131 — `review.ci_gate`
  is `null` (BR-D14), so it correctly said this repo has no review gate and stopped before
  touching the PR.

## Next

**Model: `sonnet` / coder.**

1. **Land `glunk-works/global-bootstrap#17`'s fix** (a companion IAM statement conditioned on
   `aoss:index`, or the `StringLike` fallback it names if that value format doesn't hold).
2. **Re-run `S2` Task 4 step 3's full cycle**, this time expecting a clean build: push to `main`
   queues a `tofu-apply`, click the `production` Environment approval, expect all 12 resources
   (not 8), then dispatch `destroy-ai-lab` with the typed confirm phrase (BR-D25) — the clean
   cycle #17's own Acceptance section requires. Both steps are still human-gated.

   Full spec: `grep -n '^### Task 4' -A 60 sprints/S2_identity_least_privilege/sprint_plan.md`.

**This session parks `S2`** (swapping back to `SD`) rather than chasing #17 now — that fix lives
in a different repo and isn't part of this canary. `/way-of-working:unpark-sprint S2` once #17
has landed.

## Open gates and blockers

**HITL Gate: OPEN.** Blocked on `glunk-works/global-bootstrap#17` landing, then the same two
human-gated actions as before (approval on re-apply, confirm phrase on re-destroy).
**No unattended action here.**

## Pointers

- `docs/hardening_roadmap.md` — reference of record and threat model. `BR-D14` ties
  `architect-review` readiness to `S2` landing — this blocker is part of why `S2` hasn't
  landed yet.
- `sprints/S2_identity_least_privilege/sprint_plan.md` — Task 4 step 3: 3.1 satisfied, 3.2 ran
  partially (8/12, then failed), 3.3 ran against the partial build (destroy succeeded), full
  clean cycle pending `global-bootstrap#17`.
- `glunk-works/global-bootstrap#17` — the blocking upstream issue.
- `.ai/parked/` — about to also hold `S2`'s snapshot alongside the swap back to `SD`.
