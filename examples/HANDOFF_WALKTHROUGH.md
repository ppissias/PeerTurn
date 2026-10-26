# Collaboration templates — setup and sample handoff lifecycle

These sample files are based on **`AGENT_COLLABORATION.md` v2.3** as provided by the maintainer. They are deliberately **inactive** until the maintainer authorizes Agent A to initialize a real collaboration. The project references and task IDs are illustrative; replace them with verified project paths, tasks, tests, and Git SHAs.

## Files

- `AGENTS.md` — stable project guidance, paths to specs, coding/testing expectations; read **first** at session startup.
- `AGENT_STATE.md` — feature scope, task/dependency ledger, completed tasks, pending maintainer decisions; read at task selection and transitions.
- `AGENT_HANDOFF.md` — compact, single current turn/owner; check the **committed** version on Git handoffs. Keep roughly 10–20 lines.
- `AGENT_HANDOFF_ARCHIVE.md` — append-only snapshots of every new handoff, archived **before** updating the current handoff and committed **together**.

`AGENT_COLLABORATION.md` is the authoritative collaboration protocol and is included in this release. Use all coordination files together.

## Bootstrap

1. Maintainer gives Agent A the authorized scope and assigns each agent a fixed identity A/B/C/D.
2. Agent A reads `AGENTS.md` → `AGENT_COLLABORATION.md` → `AGENT_STATE.md` → `AGENT_HANDOFF.md`.
3. Agent A checks branch/worktree and existing active collaboration. If one is already running, **do not overwrite or reinitialize**.
4. For a new run, Agent A fills real task ledger, actual base SHA and branch, then initializes the handoff with `Owner: A`, `Phase: SELECT`. First append the new full handoff to `AGENT_HANDOFF_ARCHIVE.md`, then update `AGENT_HANDOFF.md`; commit these files with state in one coordination commit. Start the first eligible task.
5. Other agents wait read-only and process committed handoffs addressed to their own fixed identities.

## Example A implements, B reviews

*These are examples to explain field values, not live handoffs and not evidence of actual tests.*

**A commits implementation code** at `<IMPLEMENTATION_SHA>`, separately from the handoff. It first appends the complete new handoff record to `AGENT_HANDOFF_ARCHIVE.md`, then writes the same record to `AGENT_HANDOFF.md`, and commits both files together:

```markdown
- **Handoff ID:** `000002`
- **Timestamp UTC:** `2026-10-26T09:00:00Z`
- **Owner:** `B`
- **Phase:** `REVIEW`
- **Task:** `DEMO-03`
- **Implementer:** `A`
- **Reviewer:** `B`
- **Task base commit:** `<BASE_SHA>`
- **Implementation commit:** `<IMPLEMENTATION_SHA>`
- **FIX rounds:** `0/2`
- **Checks:** `unit tests PASS; integration NOT RUN`
- **Last status:** `ready for review`
- **Next action:** `B reviews the complete task diff BASE..IMPLEMENTATION and records findings.`
```

For **every** later handoff (including a 30-minute owner status update), first append its *new* snapshot to the archive and then commit archive + current handoff together. If B requests changes, it commits **Owner: A**, **Phase: FIX**, grouped actionable findings and `FIX rounds: 1/2`. A fixes, runs checks, separately commits code, and returns **Owner: B**, **Phase: REVIEW**. A total of two FIX rounds is permitted per task.

If B **approves**, it updates `AGENT_STATE.md` with `DEMO-03: DONE` and reviewed SHA, then commits **Owner: B**, **Phase: SELECT**, **Reviewer: C**. B chooses the next eligible authorized task as implementer; C reviews it.

If B finds a **substantive unresolved dispute**, it commits `DEFER_REQUESTED`, transferring ownership back to A. A preserves its implementation under a dedicated Git reference and reverts only safely isolated, unapproved implementation commits using non-destructive revert commits. A records `DEFERRED` and a concrete maintainer question in `AGENT_STATE.md`, then commits ownership to B for the next independent task. If safe isolation is impossible, set `Owner: MAINTAINER`, `Phase: WAIT_MAINTAINER`; **do not continue on contaminated code**.

## Stalled agents and long-running work

- On active sessions, check the latest commit touching `AGENT_HANDOFF.md` about every **2 minutes**, using a read-only Git check. This does *not* guarantee a CLI session remains alive; autonomous wake-up depends on the actual CLI or supervisor.
- An owner doing long work commits a short status heartbeat every **30 minutes**.
- After at least **one hour without an owner commit**, follow the protocol's carefully constrained inactivity/bypass procedure. Time alone is not proof of quota exhaustion or a dead session. Never introduce concurrent writers.
- **Never bypass `Owner: MAINTAINER`.**

## Notes on the external quota monitor

The provided v2.3 protocol does **not** specify the optional `AGENT_QUOTAS.json` integration. If using the separate quota-aware extension, add its exact shared output path and behavior to the canonical collaboration protocol. The JSON and success of a small CLI probe should never be treated as proof that a particular coding session is alive.
