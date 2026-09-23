# Next steps — dev-workflow cursor

Thin, live cursor for whoever picks up this repo next. Points into the deep record
(`docs/hardening_roadmap.md`, the sprint plans, the issues) — it does not copy them.
Regenerate this at the end of every working session.

## Now

**`SD`, `implementing`.** The GitHub MCP evaluation thread is **closed — the server was added**.
`SD` Tasks 1–2 are merged; Tasks 3–4 are the active work. `S2` Task 4 continues as a parallel,
human-gated thread.

⚠️ **Restart Claude Code before doing anything else.** The `way-of-working` plugin pin has moved
twice since the last restart: `v0.5.1` → `v0.6.0` ([PR #122](https://github.com/glunk-works/bedrock-serverless-rag/pull/122),
merged) → `v0.8.0` ([PR #129](https://github.com/glunk-works/bedrock-serverless-rag/pull/129),
merged 2026-09-23). Both that bump and the GitHub MCP tools registered at `local` scope (see
below) only load in a fresh process — a session that started before either change stays on
stale skills/tools no matter what the repo now says.

## Just done

- **unparked `SD` at `03d8074`; parked `S2`:** Blocked on `glunk-works/global-bootstrap#17`
  (upstream IAM fix needed before the `S2` Task 4 step 3 cycle can complete cleanly); swapping
  back to `SD` to finish the v0.8.0 park/unpark canary rather than chase the upstream fix now.
  `S2`'s own snapshot (`.ai/parked/S2-*`) carries the full incident narrative — the queued
  apply from PR #129 was approved, built 8/12 resources, then 403'd on an AOSS index-level
  access policy; `destroy-ai-lab` then tore the partial build down cleanly.
- **GitHub MCP server added** — `github/github-mcp-server` v1.9.0, digest-pinned, authenticating
  as a **GitHub App** (`bedrock-rag-mcp-reader`) with a read-only PEM mounted into the container.
  Registered at **`local` scope; nothing committed**. Surface is 6 read-only tools
  (`get_job_logs`, `pull_request_read`, `search_pull_requests`, `actions_get`, `actions_list`,
  `list_pull_requests`), verified live — `--read-only` measurably strips 14 write tools
  (33 → 19), `--lockdown-mode` is on, and the installation grant is `actions`/`contents`/
  `metadata`/`pull_requests` all `:read`, `repository_selection: selected`.
- **Three credential designs were rejected before that one**, and the reasoning is the point:
  a PAT in a persistent env var (plaintext in `HKCU\Environment`, inherited by every child
  process); a PowerShell+DPAPI wrapper (built and verified, then deleted — Windows-bound by
  construction); and **OAuth** (cleanest storage — in-memory, PKCE, nothing on disk — but
  GitHub OAuth scopes are *account-wide*, every tool needs `repo`, and `--read-only` filters
  only the *tool list*, not the token). **Principle: bound the credential's authority first,
  then protect its storage.**
- **`.mcp.json` was written and then deleted deliberately** — a committed project-scope config
  would ship **broken** into the devcontainer (no `pwsh`, no docker-in-docker, `USERPROFILE`
  empty on Linux) and to every contributor on a public repo.
- **`way-of-working` pinned to `v0.6.0`** ([PR #122](https://github.com/glunk-works/bedrock-serverless-rag/pull/122),
  **merged**, all 6 required checks green). Root-caused why the earlier bump never took effect:
  install records duplicate by **drive-letter case** (`c:\` on `0.5.1`, `C:\` on `0.6.0`), and
  `claude plugin update -s project` matched the upper-case record and reported a **false green**.
  Corrected by hand; reported at [claude-workbench#36](https://github.com/glunk-works/claude-workbench/pull/36#issuecomment-5293546340).
- **`way-of-working` bumped again to `v0.8.0`** ([PR #129](https://github.com/glunk-works/bedrock-serverless-rag/pull/129),
  **merged** 2026-09-23). v0.7.0 added sprint archival (`/way-of-working:archive-sprint`,
  `bin/entry-anchor.sh` fixtures, a fixture-coverage Definition-of-Done requirement); v0.8.0
  added the `architect-review` skill / `review-gate` predicate and parked-sprint support
  (`/way-of-working:park-sprint`, `/way-of-working:unpark-sprint`, `.ai/parked/*` cursor-sync
  classification). Neither release added, removed, or renamed an `.ai/project.yml` key —
  `review.ci_gate` stays `null` on purpose (BR-D14), so skills still take the no-gate branch;
  `architect-review` is available but not wired to a required check in this repo.
- **Canary re-checked:** [anthropics/claude-code#16299](https://github.com/anthropics/claude-code/issues/16299)
  is still OPEN, no fix, last activity 2026-04-15 — `.claude/rules` path-scoping stays off the table.

## Next

**Model: `sonnet` / coder.**

1. **SD Task 3** — run every `gates.green` entry plus `tflint --recursive` / `checkov -d .` /
   `zizmor .github/workflows/` **inside the container from a fresh clone** (not the
   host-mounted workspace — its `.terraform/` is already initialized against the S3 backend).
   Compare verdicts to the same-commit CI run; record divergence in `docs/hardening_roadmap.md`.
   **`checkov` is expected to fail** — it fails identically on #118/#119/#120/#122 and is
   deliberately not a required check (F6/F7). A container-vs-CI *divergence* is the finding.
   Spec: `grep -n '^- \*\*Task 3' -A 60 sprints/SD_devcontainer/sprint_plan.md`.
2. **SD Task 4** — `.devcontainer/README.md`, BR-D15 in the roadmap, a `CLAUDE.md` pointer, a
   comment-only note in `.ai/project.yml`. While in the roadmap, also close two carried items:
   the **F61 row** (SD Task 1 closed *both* halves) and a **decision record for the GitHub MCP
   server** (App auth, why not PAT/OAuth, the digest pin, devcontainer incompatibility).
3. **S2 Task 4 step 3** — sub-steps 3.2/3.3, unchanged, **human-gated**.

Also worth doing next session: the **canary run** upstream owes `claude-workbench#36` a full
`/way-of-working:resume` → `/way-of-working:ship` pass against a *loaded* plugin — now `v0.8.0`,
not the `v0.6.0` originally promised, since the pin moved again before that pass ran; stage 2
fan-out is waiting on it.

## Open gates and blockers

**HITL Gate: OPEN.** `parked 2026-09-23 at 7bf3209; re-verify next_action against what merged
since: git log 7bf3209..HEAD --oneline` — this cursor was just restored from a park, so a human
re-verifies **Next** below is still accurate before `/way-of-working:resume` may auto-start it.
`S2`'s own thread is separately parked (see Pointers) and not part of this gate.

**Process notes new this session** (both saved to memory):
- **Git Bash rewrites Unix-looking absolute paths into Windows paths.** It broke
  `gh api /orgs/...` loudly *and* silently corrupted a container-internal path in a persisted
  MCP config — exit 0, wrong value on disk. Prefix `MSYS_NO_PATHCONV=1` and **verify what was
  actually recorded**.
- **PowerShell swallows a bare `--`** before a native command, and `--%` doesn't help against a
  shim like `claude`. Run those from Bash.
- **Untested repair path:** the documented `marketplace add @ref` / `uninstall` / `install`
  sequence from the *lower-case* cwd may fix the case-duplicated plugin record through
  supported commands — try that before hand-editing next time. Backup left at
  `~/.claude/plugins/installed_plugins.json.bak` (harmless, delete at will).

## Pointers

- `docs/hardening_roadmap.md` — reference of record and threat model. Untouched this session;
  two edits queued above.
- `sprints/SD_devcontainer/sprint_plan.md` — Tasks 1–2 done, Tasks 3–4 next.
- `sprints/S2_identity_least_privilege/sprint_plan.md` — Task 4 step 3: 3.1 satisfied,
  3.2/3.3 pending human action.
- `.ai/parked/` — holds `S2`'s snapshot (`parked_at` 2026-09-23, blocked on
  `glunk-works/global-bootstrap#17`). Restore with `/way-of-working:unpark-sprint S2` once
  that upstream fix lands.
