# Next steps — dev-workflow cursor

Thin, live cursor for whoever picks up this repo next. Points into the deep record
(`docs/hardening_roadmap.md`, the milestones and issues, the archived sprint plans) — it does
not copy them. Regenerate this at the end of every working session.

## Now

**`SD`, `implementing`** — [milestone #1](https://github.com/glunk-works/bedrock-serverless-rag/milestone/1).
Tasks 1–3 are merged; **Task 4 (#137)** and Task 5 (#138) remain. `S2` is parked on
[milestone #2](https://github.com/glunk-works/bedrock-serverless-rag/milestone/2), blocked on
`glunk-works/global-bootstrap#17`.

**The sprint ledger is now GitHub milestones and issues, not files.** `.ai/project.yml`
declares `planning.kind: github_milestones` (PR #136); this session made the repo agree with it.

## Just done

- **Migrated the file-based sprint ledger to GitHub milestones** (2026-09-30, Fable/architect):
  five milestones — #1 `SD`, #2 `S2`, #3 `S3+S4`, #4 `S5`, #5 `S6` — each with a description in
  the plan-sprint template shape (goal, build order, *Deliberately NOT here*, `BLOCKING:`, model
  per phase, DoD); **18 task issues** for the *remaining* tasks only (#137–#155 less #154,
  folded into #155 per S6's "T4/T5: MERGE" banner; plus #8 reused as `S6`-T1 with its spec
  added as a comment). Every issue body is the full task spec **rewritten to agree with its
  sprint's banner** — the BR-D23 cuts are now applied in the executable text (S5 went from
  four new required checks to one, keeping the banner's four items and its original task ids
  in the titles; S3-T1 lost its VPC branch; S4-T4 lost the retry half `MW`-T3 already fixed).
- **`/way-of-working:critic-gate` ran `docs-consistency` on the migration** (the only critic look
  this diff gets — `review.ci_gate` is `null`): round 1 returned 14 findings, nine of them
  false or operationally wrong claims *in the issue bodies* (BR-D15 "add" when it already
  exists; a tflint rule "re-enable" already done; a checkov "no suppression" criterion that
  cannot pass beside the cuts; a retry hazard contradicting PR #93; #139's step-4 deletion
  list missing `state_kms_access_policy`; S5 renumbered under the roadmap's cites; F62 dropped
  from #137; `test_rag.py`'s leak misdescribed; pytest routed into the apply job's requirements).
  All verified against the source, all fixed in the issues and the tree. **4 rounds total —
  initial pass + 3 fix-and-re-run rounds — converged** (14 → 10 → 5 → 4 findings, severity
  falling each round; rounds 3 and 4 tightenings only), all on `docs-consistency`'s own
  default model (`opus`); the one-shot second-opinion round on `fable` was **offered and
  declined**. Four optional tightenings from round 4 are accepted-with-reason, not applied
  (each is already covered by an observed-output criterion or is wording). Every fix round
  edited GitHub text, so the SD anchor was rewritten after the last edit; both anchors verify
  `match`.
  **Completed tasks were not backfilled.** The four finished sprints' plans stay at `sprints/`;
  the six migrated plans moved to `sprints/_archive/` with a superseded banner, kept for their
  Critical reviews and residual registers. Roadmap § 5 rows link their milestones; § 10 has the
  entry; `CLAUDE.md`'s pointer explains the new split; `load_bearing_docs` drops the plan glob.
- **Cursor re-anchored:** `pointers.sprint_plan` is the milestone URL and `pointers.plan_anchor`
  was written by `plan-anchor.sh write` on #137 (SD-T4); `S2`'s parked snapshot likewise on #139.
- **Resume housekeeping this session:** the checkout was on an already-merged branch and behind
  `main` by #135/#136 — fast-forwarded, stale branch deleted; pruned 3 squash-merged local
  branches, **skipped `docs/sync-cursor-github-mcp-and-pin`** (its tip is not what GitHub merged
  — stranded work, look before deleting); ruleset healthy (4 rule types, 6 required checks).

## Next

**Model: `sonnet` / coder.**

1. **task #137 — SD-T4:** `.devcontainer/README.md` with CI's exact command lines (closes
   **F62**), update the *existing* BR-D15 row in roadmap § 4 to in-force, the `CLAUDE.md`
   § Commands pointer, a comment-only note in `.ai/project.yml`, and the two carried roadmap
   edits (correct the stale **F61** row; record the **GitHub MCP server** decision). Spec is the
   issue body; the archived plan's Critical review is
   `sprints/_archive/SD_devcontainer/sprint_plan.md`.
2. **task #138 — SD-T5:** dependabot `docker` entry, no per-PR image build.
3. **`/way-of-working:plan-sprint`** for the eight open issues deliberately left unmilestoned
   (#74, #95, #96, #100, #101, #121, #132, #134) — that skill's job, not the migration's.
4. **`S2`** stays parked until `global-bootstrap#17` lands; then `/way-of-working:unpark-sprint S2`
   → #139.

## Open gates and blockers

**HITL Gate: OPEN.** Two things, in order: **(a)** the migration PR must merge — until it does,
`main` still says the plans are files and `pointers.sprint_plan` points at a milestone `main`
does not describe; the first `/way-of-working:resume` after the merge will classify HEAD vs
`last_commit` as **`drift`** (the PR touches far more than the ledger) — **that is expected, once**,
and the human re-verifies rather than auto-starting. **(b)** Then re-verify that **#137** is still
the right next action (this session confirmed Tasks 1–3 shipped — PRs #117, #118, #124 — and the
2026-08-14 handoff queued exactly this task). Once both hold, the next handoff writes
`NONE OPEN`. `S2`'s own gate is separately OPEN in its parked snapshot and is not part of this one.

**Process notes this session:**
- The migration wrote to GitHub **before** the PR existed (milestones and issues have no
  branch). That is unavoidable under this kind and is why the plan anchor exists — but it means
  a reverted PR would leave live milestones behind. Close them by hand if that ever happens.
- `plan-anchor.sh write` must run **after** the last description edit; an anchor taken before a
  PATCH reads as `drift` forever. Both anchors here were written after the final PATCH.

## Pointers

- `docs/hardening_roadmap.md` — reference of record and threat model. § 5 table links each
  milestone; the 2026-09-30 § 10 entry records the migration and what it trades.
- [Milestone #1 `SD`](https://github.com/glunk-works/bedrock-serverless-rag/milestone/1) — the
  live sprint: #137, #138.
- [Milestone #2 `S2`](https://github.com/glunk-works/bedrock-serverless-rag/milestone/2) —
  parked (`.ai/parked/S2-*`, `parked_at` 2026-09-23): #139, #140, #141.
- Milestones #3 `S3+S4` (#142–#148), #4 `S5` (#149–#152), #5 `S6` (#8, #153, #155) — planned,
  ordered by `due_on` (display order only).
- `sprints/_archive/` — the six migrated plans; `sprints/` — the four completed ones. Record,
  not plan.
