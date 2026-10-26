# PeerTurn

**A tiny, Git-native collaboration protocol for AI coding agents.**

*Implement → Review → Handoff → Rotate → Continue.*

PeerTurn is an experimental way to let **two or more independently running coding agents** work through an authorized backlog together. It uses ordinary **Markdown files and Git commits** to communicate who is implementing, who is reviewing, what is finished, and what needs a maintainer's decision.

There is no mandatory server, framework, database, MCP integration, or dedicated orchestration process. You bring your own CLI agents, start them in separate terminals in **the same Git working tree**, and give each a fixed identity: `Agent A`, `Agent B`, `Agent C`, and so on.

> [!IMPORTANT]
> This is a hobby/experimental workflow, **not a reliable unattended agent runtime**. It has worked in informal tests, but CLI sessions can exit unexpectedly, agents can lose their identity or context, and Git-only inactivity detection cannot prove that a process has stopped. **Review all changes before integrating them.**

## The idea in 30 seconds

```text
Authorized task backlog
        |
        v
    Agent A implements task 1
        |
        v
    Agent B reviews task 1
        |  \-- requests fixes -> Agent A fixes -> Agent B re-reviews
        |  \-- unresolved dispute -> safely defer task 1
        v
    Agent B implements task 2
        |
        v
    Agent C reviews task 2
        |
        v
    Agent C implements task 3 -> Agent D reviews -> Agent D implements ...
```

The reviewer becomes the **next implementer** after approval or safe deferral. With only two agents, roles alternate `A/B → B/A → A/B`.

The work is **sequential, not parallel**: in a shared working tree, only the agent named as `Owner` in the **committed** `AGENT_HANDOFF.md` is allowed to write. Other agents are read-only until ownership transfers via a committed handoff.

## What's included

| File | Purpose |
| --- | --- |
| [`AGENT_COLLABORATION.md`](AGENT_COLLABORATION.md) | The authoritative v2.3 protocol: safety, rotation, review, archiving, polling and recovery rules. |
| [`AGENTS.md`](AGENTS.md) | **Inactive template** for a target project's architecture, feature locations, coding and test rules. Agents read it first. |
| [`AGENT_STATE.md`](AGENT_STATE.md) | **Inactive template** for the authorized backlog, completed work, dependencies and maintainer decisions. |
| [`AGENT_HANDOFF.md`](AGENT_HANDOFF.md) | **Inactive template** for the current owner, phase, task and exact next action. |
| [`AGENT_HANDOFF_ARCHIVE.md`](AGENT_HANDOFF_ARCHIVE.md) | **Append-only template** for each full new handoff record (including status updates). |
| [`examples/HANDOFF_WALKTHROUGH.md`](examples/HANDOFF_WALKTHROUGH.md) | A worked example of bootstrap, review, fixes, deferral and rotation. |

The four example templates are deliberately **not active tasks**. Their `DEMO-*` entries and placeholder SHAs are illustrations, not real progress or permission to write code.

## Quick start

### 1. Copy the five Markdown files to your own repository

In the **target software project**, put these files at its root:

```text
my-project/
  AGENTS.md
  AGENT_COLLABORATION.md
  AGENT_STATE.md
  AGENT_HANDOFF.md
  AGENT_HANDOFF_ARCHIVE.md
  src/...
```

Edit `AGENTS.md` to accurately describe that **target project**, including its real feature specifications, build commands and tests. Do not blindly overwrite an existing `AGENTS.md`: merge the relevant guidance instead. Keep `AGENT_STATE.md` and `AGENT_HANDOFF.md` inactive until you explicitly authorize Agent A to initialize the collaboration. `AGENT_HANDOFF_ARCHIVE.md` starts empty and is append-only after bootstrap.

You need Git and the coding CLIs of your choice. The examples below use four agents, but **two agents are enough**.

### 2. Start separate CLI sessions in the same working directory

For example, open four terminal tabs in `my-project` and start a different supported coding CLI in each. In informal use, this setup has been tried with **Grok, Claude Code, Codex and Gemini** (including Gemini-based tooling). This is not an official integration or a compatibility certification; available CLI commands and behavior vary by version.

The identity letters are **roles within PeerTurn**, not provider names. You choose which CLI is A, B, C or D, and the role stays fixed for that session.

### 3. Give Agent A the goal and authorize bootstrap

Example initial prompt for **Agent A**:

```text
You are Agent A. Your identity is A for this entire session;
do not derive or change it from AGENT_HANDOFF.md.

First read AGENTS.md, then AGENT_COLLABORATION.md,
AGENT_STATE.md and AGENT_HANDOFF.md.

Goal: Add input validation, improve cancellation handling,
and add regression tests for our import pipeline.
Keep behavior and existing public APIs compatible.

You are authorized to initialize the collaboration from a
clean, safe Git state. Create a bounded backlog and acceptance
criteria in AGENT_STATE.md, append the new handoff snapshot to AGENT_HANDOFF_ARCHIVE.md,
then commit the matching AGENT_HANDOFF.md and archive together.
Implement the first eligible task. Follow the protocol.

Stay active while your CLI supports it. While idle, poll for
new committed handoffs every 2 minutes. Never assume you are
someone else because of the Owner field.
```

Replace the example goal with the work you actually want. The agent must use the project's real requirements and tests; it must not invent extra features outside the authorized scope.

### 4. Join the other agents

Example prompt for **Agent B** (adapt the letter for C and D):

```text
You are Agent B. Your permanent identity in this collaboration
is B. Never confuse your identity with the current Owner.

First read AGENTS.md, then AGENT_COLLABORATION.md,
AGENT_STATE.md and the committed AGENT_HANDOFF.md.

Join the active collaboration. Do not reinitialize it.
If the committed Owner is not B, remain read-only and check
Git for a new handoff every 2 minutes while your CLI supports it.
When a committed handoff assigns ownership to B, act exactly
once for that handoff and follow the protocol. When handing over,
archive the new handoff snapshot first and commit both files
together. Keep working
through eligible tasks and handoffs, without requiring me to
prompt you for every turn.
```

You can repeat the same instructions for C and D. Saying **“You are Agent B”** explicitly is useful: in actual tests, a session sometimes lost track of its identity and waited for “Agent B” even though *it was* Agent B.

### 5. Watch the progress, then review it yourself

`AGENT_STATE.md` becomes the compact overall report. `AGENT_HANDOFF.md` is just the current mailbox; `AGENT_HANDOFF_ARCHIVE.md` and Git history preserve earlier handoffs. Inspect both files, tests, and code changes before merging anything.

The workflow can be fun to leave running during a break or overnight, but treat the result as **untrusted work requiring maintainer review**. Monitor critical work and keep destructive permissions constrained.

## Example of a real handoff

After Agent A separately commits code, it prepares a short mailbox update like this **and appends the exact new record to `AGENT_HANDOFF_ARCHIVE.md` before writing the active handoff**:

```markdown
- **Handoff ID:** `000012`
- **Timestamp UTC:** `2026-10-26T09:00:00Z`
- **Owner:** `B`
- **Phase:** `REVIEW`
- **Task:** `IMPORT-01`
- **Implementer:** `A`
- **Reviewer:** `B`
- **Task base commit:** `<BASE_SHA>`
- **Implementation commit:** `<IMPLEMENTATION_SHA>`
- **FIX rounds:** `0/2`
- **Checks:** `unit tests PASS; integration NOT RUN`
- **Last status:** `ready for review`
- **Next action:** `B reviews the complete IMPORT-01 diff.`
```

The example handoff ID and timestamp are illustrative, and the commit IDs are placeholders, **not real Git SHAs**. The archive and updated handoff must be committed **together** before Agent B can act. Agent B reviews the code from the recorded base, then either:

- **Approves** it, marks `IMPORT-01` `DONE`, and becomes the implementer of the next eligible task (reviewed by C).
- **Requests fixes** from A (up to two grouped FIX rounds for the task), then re-reviews A's corrections.
- **Defers** a significant unresolved disagreement: the implementation is preserved and safely isolated before the team proceeds to an independent task. The decision and differing views are recorded for the maintainer.

A `DEFERRED` task is not `DONE`. If isolating unapproved code is unsafe, the agents must stop and ask the maintainer rather than continuing with a contaminated working tree.

## Why Git + Markdown?

- **Small amount of inter-agent text:** point to the commit and specification instead of re-explaining the entire diff.
- **Durable handoffs:** ownership transfers only when the current handoff and its matching append-only archive entry are committed together.
- **Independent reviews:** every implementation is examined by a different agent.
- **Visible decisions:** the maintainer can see what was approved, deferred or blocked.
- **No vendor lock-in at the protocol level:** agents only need to read files, inspect Git and follow the agreed rules.

## Staying active and recovering from failures

The current v2.3 protocol specifies:

| Mechanism | Protocol expectation |
| --- | --- |
| Idle polling | Check Git for new `AGENT_HANDOFF.md` commits about every **2 minutes** (not the full archive). |
| Append-only history | Archive **every new handoff snapshot first**, then update the active handoff and commit both together. |
| Long-running owner status | Commit a short progress update at least every **30 minutes**. |
| Suspected inactivity | After **1 hour without an owner commit**, follow the strictly limited bypass procedure in the protocol. |
| Shared-worktree protection | Only the committed owner writes; never overwrite unknown uncommitted work. |
| Human intervention | Use `Owner: MAINTAINER` for situations that cannot be continued safely. |

**These are instructions, not an operating-system scheduler or process supervisor.** A prompt cannot force a closed CLI to restart, and a Git commit timestamp cannot prove that a particular process has exited. The one-hour bypass is especially experimental: before any takeover, ensure the previous process cannot still write. Where this cannot be established, **do not attempt concurrent takeover**.

### What has happened in practice

In informal hands-on testing with recent Grok, Claude Code, Codex and Gemini versions, the workflow has generally progressed through multiple tasks and reviews. It is **not very robust yet**, however:

- A CLI sometimes exits or becomes unavailable, including during memory pressure.
- A CLI stops because it has exaust its quota / tokens. 
- Sessions can stop polling or lose context, especially after long periods.
- Occasionally an agent forgets its assigned identity (for example, Agent B says it is waiting for Agent B).
- A waiting agent may not be reawakened automatically even when the next handoff has been committed.
- Long-running tools/tests may take time without a new Git commit; silence alone is not proof of failure.

If a session ends, restart it explicitly with its original identity, point it to the **current committed handoff**, and check ownership before resuming. Do **not** reuse stale session assumptions.

For truly unattended execution, a separate supervisor that can observe processes, stop old writers and safely wake the appropriate CLI would be needed. That is **not included** in this repo.

## Safety and scope

Read the full [`AGENT_COLLABORATION.md`](AGENT_COLLABORATION.md) before using this protocol. In particular:

- Only implement **maintainer-authorized**, clearly bounded work.
- Never allow two agents to modify the shared working tree simultaneously.
- Do not let a reviewer modify the implementation code it is reviewing.
- Do not automatically push, merge or deploy; these remain maintainer decisions.
- Do not use destructive Git shortcuts to recover from an uncertain situation.
- Preserve unresolved code and record maintainer decisions without pretending that deferred work is complete.
- Run and report the project's actual checks; never invent test evidence.

## Status and contributions

This is a **small experimental protocol**, not a mature framework. If you try it with other agent combinations, useful feedback includes identity/context failures, incorrect ownership handoffs, long-running polling behavior and safe recovery procedures.

