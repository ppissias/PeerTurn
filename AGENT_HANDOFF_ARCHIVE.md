# AGENT_HANDOFF_ARCHIVE.md — Append-Only Handoff History (TEMPLATE)

> **INACTIVE TEMPLATE.** The archive starts empty. On EVERY update to `AGENT_HANDOFF.md`, the authorized writer **first appends the complete NEW handoff snapshot here**, then replaces `AGENT_HANDOFF.md` with that exact snapshot, and commits both files **in the same Git commit**. Do not edit or remove earlier entries. The current committed handoff—not this archive—controls ownership.

<!--
Append new records below this comment, oldest first. Use the next unique Handoff ID
across this repository; do not reset IDs between tasks. Do not read the whole archive
on routine 2-minute polls. This is for human audit and recovery.

EXAMPLE ENTRY FORMAT (do NOT consider this sample a real handoff):

### Handoff 000012 — 2026-10-26T09:00:00Z

- **Recorded by:** `A`
- **Reason:** `VALIDATE-01 implementation ready for B to review`

```markdown
# AGENT_HANDOFF.md — Current Turn

- **Handoff ID:** `000012`
- **Timestamp UTC:** `2026-10-26T09:00:00Z`
- **Owner:** `B`
- **Phase:** `REVIEW`
- **Task:** `VALIDATE-01`
- **Implementer:** `A`
- **Reviewer:** `B`
- **Task base commit:** `<ACTUAL_SHA>`
- **Implementation commit:** `<ACTUAL_SHA>`
- **FIX rounds:** `0/2`
- **Checks:** `unit tests PASS; integration NOT RUN`
- **Last status:** `code committed; ready for review`
- **Next action:** `B reviews full diff BASE..IMPLEMENTATION and returns approval, fixes, or deferral.`
```

The fenced snapshot MUST be copied verbatim to AGENT_HANDOFF.md. Only after both
files are committed together may B take ownership. A repeats this archive-first
operation even for status-only handoffs; do not duplicate an existing Handoff ID.
-->
