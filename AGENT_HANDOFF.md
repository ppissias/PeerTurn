# AGENT_HANDOFF.md — Current Turn (TEMPLATE)

> **INACTIVE TEMPLATE — NOT AN AUTHORIZATION TO WORK.** The maintainer must authorize Agent A to bootstrap. All handoff updates are archived **first** in `AGENT_HANDOFF_ARCHIVE.md`, then written here, and both files are committed **together**. Only the committed handoff determines ownership.

- **Handoff ID:** `TEMPLATE`
- **Timestamp UTC:** `NOT SET`
- **Owner:** `UNASSIGNED`
- **Phase:** `TEMPLATE`
- **Task:** `NONE`
- **Implementer:** `NONE`
- **Reviewer:** `NONE`
- **Task base commit:** `NONE`
- **Implementation commit:** `NONE`
- **FIX rounds:** `0/2`
- **Checks:** `NOT RUN`
- **Last status:** `NOT STARTED`
- **Next action:** `Maintainer authorizes Agent A; A initializes state, archives handoff 000001, and commits SELECT ownership.`

<!--
AGENT INSTRUCTIONS — HOW TO UPDATE THIS FILE:
1. Read AGENTS.md, AGENT_COLLABORATION.md, AGENT_STATE.md, and the committed handoff.
2. Confirm your own maintainer-assigned ID is the committed Owner; confirm clean/safe Git status.
   Exception: first bootstrap explicitly authorized for A, or the strictly limited inactive-owner bypass.
3. Construct the COMPLETE replacement snapshot using the field names above. Keep it ~10–20 lines.
4. Allocate the next unique Handoff ID (000001, 000002, ...), UTC timestamp, and exact next action.
5. APPEND this complete new snapshot to AGENT_HANDOFF_ARCHIVE.md FIRST (include ID,
   recorded-by, UTC timestamp and short reason); preserve previous archive entries.
6. Replace this file with exactly the snapshot you just appended. Commit BOTH files together.
   If also changing status or decisions, include AGENT_STATE.md in that same coordination commit.
7. Ownership changes only after successful commit. Sender stops writing immediately afterward.
   Code changes must be committed separately BEFORE the handoff commit.
8. Even a 30-minute status-only heartbeat requires a new ID and matching archive append.
9. Never append a running transcript here; put only current state and next action.

EXAMPLE REVIEW handoff (illustrative, NOT live; archive the entire snapshot before writing it):
  Handoff ID: 000012                 Timestamp UTC: 2026-10-26T09:00:00Z
  Owner: B                           Phase: REVIEW
  Task: VALIDATE-01                  Implementer: A           Reviewer: B
  Task base commit: <ACTUAL_SHA>    Implementation commit: <ACTUAL_SHA>
  FIX rounds: 0/2                   Checks: unit tests PASS; integration NOT RUN
  Last status: code committed       Next action: B reviews complete diff and responds

OTHER PHASES:
  SELECT: owner is next implementer; next action selects an eligible task from AGENT_STATE.md.
  FIX: owner is original implementer; reviewer records one grouped actionable findings summary.
  REVIEW: owner is reviewer; full task diff/validation is examined independently.
  DEFER_REQUESTED: owner is original implementer, who safely isolates disputed code.
  WAIT_MAINTAINER: Owner MAINTAINER; specify blocker and requested decision.

APPROVED and safe DEFERRED are outcomes recorded in AGENT_STATE.md and the archived
handoff. Active Phase normally becomes SELECT with next implementer as Owner.
-->
