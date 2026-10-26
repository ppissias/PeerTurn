# AGENT_STATE.md — Project Backlog and Decisions (TEMPLATE)

> **INACTIVE TEMPLATE.** All `DEMO-*` rows and decisions are illustrative, not authorized work or evidence of progress. Agent A must replace them with the maintainer's actual scope during an explicitly authorized bootstrap. `AGENT_HANDOFF.md` is the single active turn; `AGENT_HANDOFF_ARCHIVE.md` records every new handoff snapshot.

## Collaboration configuration

- **Lifecycle:** `TEMPLATE_NOT_INITIALIZED`
- **Project:** `[PROJECT_NAME]`
- **Maintainer goal / authorized scope:** `[LINK_OR_DESCRIPTION_FROM_MAINTAINER]`
- **Specification / acceptance source:** `[SPEC_PATH_OR_LINK]`
- **Feature branch:** `[AUTHORIZED_BRANCH — NOT YET CREATED]`
- **Collaboration base commit:** `[REAL_BASE_SHA — CAPTURE BEFORE FEATURE CHANGES]`
- **Ordered roster:** `A → B → C → D → A` *(example; maintainer assigns actual CLI identities)*
- **Initial implementer / reviewer:** `A / B` *(after bootstrap)*
- **Integration owner:** `MAINTAINER` *(no automatic merge/push/deploy)*

## Task ledger

One row per **bounded maintainer-authorized task**. Link to the actual specification; keep only short results here. Task status is separate from the active `Owner`/`Phase` fields in `AGENT_HANDOFF.md`.

| Task | Acceptance target / specification | Dependencies | Status | Evidence / outcome |
| --- | --- | --- | --- | --- |
| `DEMO-01` | Extract shared input validator; preserve public behavior | — | `DONE` *(example)* | `base=[SHA]; reviewed=[SHA]; tests=PASS; minor change: shared error wording` |
| `DEMO-02` | Agree on navigation cancellation policy | `DEMO-01` | `DEFERRED` *(example)* | `D-01; saved=deferred/DEMO-02; base=[SHA]; tip=[SHA]` |
| `DEMO-03` | Add regression tests to validator | `DEMO-01` | `IN_REVIEW` *(example)* | `A→B; base=[SHA]; implementation=[SHA]; checks=PASS` |
| `DEMO-04` | Implement chosen cancellation behavior | `DEMO-02` | `BLOCKED_DEPENDENCY` *(example)* | `waiting for D-01; do not start` |
| `DEMO-05` | Improve unrelated logging | — | `TODO` *(example)* | `eligible; not started` |

**Replace all `DEMO-*` rows at bootstrap.** Do not turn examples into live tasks. Never mark a task `DONE` without completed independent review. If a task is deferred after code was committed, preserve its work under a dedicated Git reference and safely isolate it first, as the collaboration protocol requires.

### Allowed task statuses and required changes

| Status | When to set it | Minimum evidence to record |
| --- | --- | --- |
| `TODO` | Authorized and not started; eligible if dependencies allow | Acceptance target, spec, dependencies |
| `IN_PROGRESS` | Implementer has selected/started it | Implementer, real task baseline, planned checks |
| `IN_REVIEW` | Code separately committed and handed to reviewer | Implementer/reviewer, baseline and implementation SHA, actual test results |
| `DONE` | Reviewer approved complete agreed scope | Reviewed SHA, actual tests/limitations, any agreed deviations |
| `DEFERRED` | Real unresolved issue requires maintainer decision | Decision ID, both views, preserved Git ref/SHAs if code exists, affected dependencies |
| `BLOCKED_DEPENDENCY` | Cannot start because required task/decision remains unresolved | ID of blocking task/decision |

## Pending maintainer decisions

Only unresolved decisions belong here. Use `agent-decisions/D-xx.md` for complex reasoning; everyday tasks do not need per-task files.

### D-01 — Example: cancellation policy (NOT A REAL DECISION)

- **Status:** `OPEN` *(example)*
- **Task / dependent tasks:** `DEMO-02` / `DEMO-04`.
- **Decision needed:** Cancel running work immediately on navigation (A) or finish but suppress stale output (B)?
- **Implementer view:** A favors prompt cancellation and freeing resources; safe interruption must be ensured.
- **Reviewer view:** B favors finishing safely and suppressing stale callbacks; avoids partial-output corner cases.
- **Preserved evidence:** `deferred/DEMO-02`, base `[SHA]`, implementation tip `[SHA]`; handoff IDs in `AGENT_HANDOFF_ARCHIVE.md`.
- **Maintainer action:** Choose policy, record outcome, make `DEMO-02` eligible, and require fresh independent review.

## Completed work summary

- **DONE:** `[ACTUAL_IDS_AND_REVIEWED_SHAS_ONLY]`
- **Deferred / pending decisions:** `[ACTUAL_TASK_IDS_AND_DECISION_IDS]`
- **Blocked by dependencies:** `[ACTUAL_TASK_IDS]`
- **Still eligible:** `[ACTUAL_TASK_IDS]`
- **Final integration:** `MAINTAINER ONLY; pending until authorized`

## Update procedure for agents

1. **At bootstrap:** Agent A replaces all placeholders/examples with the real roster, branch, base commit, scope, acceptance targets and initial `TODO` ledger. Initialization requires a matching `AGENT_HANDOFF.md` snapshot archived first and committed with `AGENT_HANDOFF_ARCHIVE.md`.
2. **At task selection:** Implementer marks one eligible `TODO` task `IN_PROGRESS`, records actual baseline and checks, then continues within the current committed ownership rules. If updating `AGENT_HANDOFF.md`, append and commit its matching archive entry as well.
3. **At implementation handoff:** After a separate implementation code commit, set `IN_REVIEW` with actual SHAs and tests. Archive the new `REVIEW` snapshot **before** updating the handoff and commit both coordination files together with this state update.
4. **At review:** Reviewer marks `DONE` only after approval, records reviewed SHA, actual checks and agreed deviations; rotate to `SELECT`. For a `FIX`, keep task `IN_PROGRESS`/`IN_REVIEW` as appropriate and group findings in the current handoff. For `DEFER_REQUESTED`, wait for safe code isolation before setting `DEFERRED`.
5. **At deferral:** Record a concise decision, both views, dependencies, preserved branch/ref and real SHAs. Continue with independent eligible tasks. If no safe way to isolate unapproved code, use `WAIT_MAINTAINER` rather than pretending the task is deferred safely.
6. **At maintainer resolution:** Record the chosen decision without deleting its history, change the affected task back to `TODO` (or another justified eligible status), and request fresh independent review before `DONE`.
7. **At completion:** Do not claim the collaboration is fully finished while authorized tasks are `DEFERRED` or blocked. Present all remaining decisions and evidence to the maintainer.

**Read discipline:** Agents read this state when choosing tasks or changing outcomes; idle polling checks only the committed handoff. The append-only handoff archive is not part of every agent's routine context.
