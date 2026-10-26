# AGENTS.md — Project Guide (TEMPLATE)

> **TEMPLATE — EDIT BEFORE USE.** Replace every `[PLACEHOLDER]` with verified project information. This file does not authorize work by itself. The maintainer defines the scope; `AGENT_STATE.md` records it.
>
> **Read this file FIRST** on every agent session startup, then `AGENT_COLLABORATION.md`, `AGENT_STATE.md`, and `AGENT_HANDOFF.md`, in that order.

## 1. Project overview

- **Project:** `[PROJECT_NAME]`
- **Purpose:** `[ONE_OR_TWO_SENTENCE_PROJECT_DESCRIPTION]`
- **Primary language / runtime:** `[E.G., JAVA 21 — VERIFY]`
- **Build system:** `[E.G., GRADLE — VERIFY]`
- **Supported environments:** `[WINDOWS/LINUX/MACOS AS APPLICABLE]`
- **Main entry points:** `[ACTUAL FILES OR MODULES]`
- **Non-goals:** `[OUT_OF_SCOPE_AREAS]`

**Illustrative description (replace it):** “A desktop application that imports image datasets, processes them, and exports reports. Features must preserve deterministic outputs and safe session cleanup.” This is an example, **not** a statement about the current repository.

## 2. Repository map and sources of truth

| Topic | Actual location (fill in) |
| --- | --- |
| Feature specifications and acceptance criteria | `[PATH_TO_FEATURE_SPEC_OR_ISSUE_TRACKER]` |
| Architecture / design decisions | `[PATH_TO_DESIGN_DOCUMENTATION]` |
| Developer and testing guidance | `[PATH_TO_DEVELOPER_GUIDE]` |
| Application code | `[SOURCE_DIRECTORIES]` |
| Tests and test data | `[TEST_DIRECTORIES]` |
| User / API documentation | `[DOCUMENTATION_DIRECTORIES]` |

Do **not** infer a feature's requirements from its title. Read its specification and the relevant code. If a referenced path does not exist, do not invent or silently create it: report the missing reference.

## 3. Working on features

1. Only work on the **maintainer-authorized scope** in `AGENT_STATE.md`; do not invent additional features.
2. Respect dependencies, acceptance criteria, compatibility requirements, and the project's established design.
3. Prefer a **small, reviewable change** over broad refactoring unrelated to the task.
4. Handle errors, cancellation, cleanup, threading, and user-facing behavior as relevant to the code being changed.
5. Update specifications/docs/tests when an implemented change actually requires it.
6. Report any deviation from the specification explicitly in the review handoff and state ledger.

## 4. Checks and validation

Replace these example commands with the **actual** commands for this repository. Never claim a command was run unless it was executed and its result observed.

| Check | Command / procedure |
| --- | --- |
| Compile / build | `[BUILD_COMMAND]` |
| Focused tests | `[FOCUSED_TEST_COMMAND]` |
| Full test suite | `[FULL_TEST_COMMAND]` |
| Static checks / lint | `[LINT_OR_CHECK_COMMAND]` |
| Integration / UI tests | `[INTEGRATION_COMMAND_OR_MANUAL_PROCEDURE]` |
| Real-data / regression validation | `[REPRODUCIBLE_BASELINE_COMPARISON_IF_RELEVANT]` |

**Example only (do not assume it exists):** for a Gradle Java project, `./gradlew test` may be an appropriate unit-test command. The actual repository instructions take precedence.

- Prefer focused tests while iterating; perform the required wider checks before review when feasible.
- Describe **PASS**, **FAIL**, **NOT RUN**, and any environment limitations accurately.
- Avoid writing test artifacts to tracked directories unless the project specifically requires it.
- Never delete or overwrite user data or unrelated changes.

## 5. Git and collaboration

The detailed collaboration rules are in [`AGENT_COLLABORATION.md`](AGENT_COLLABORATION.md) and **take precedence for handoffs**. In particular:

- Each agent has a fixed identity (A, B, C, …) assigned by the maintainer. Never adopt an identity from the handoff file.
- **Single writer:** only the owner in the **committed** `AGENT_HANDOFF.md` may write. Other agents wait read-only.
- Before writing, check the actual Git branch, committed handoff, and `git status`.
- Implementation code and coordination are committed **separately**. Before **every** update to `AGENT_HANDOFF.md` (including progress heartbeats), append the exact new snapshot to `AGENT_HANDOFF_ARCHIVE.md`; commit **both coordination files together**. Ownership changes only after that commit succeeds.
- Review the entire task diff using the recorded base/implementation commits; never approve your own implementation.
- After review, either approve, request at most two grouped FIX rounds, or defer a substantive unresolved decision as instructed by the protocol.
- Use `AGENT_STATE.md` for completed work and decisions; `AGENT_HANDOFF.md` is only the *current* turn, while `AGENT_HANDOFF_ARCHIVE.md` is the append-only record of all handoffs.
- Do not reset hard, clean unknown files, automatically stash, rebase, force-push, merge, push, or deploy without the necessary authorization.

## 6. Safety and escalation

- Do not commit credentials, tokens, personal data, or generated secrets.
- An unclear or unsafe shared working tree is a **stop condition**, not permission to overwrite another participant's changes.
- Never treat elapsed time alone as proof a CLI session is dead. Follow the inactivity procedure in `AGENT_COLLABORATION.md`; do not start a second writer on an uncertain tree.
- For a design disagreement, defer the *task* and continue with other independent authorized tasks **only after** the unapproved implementation has been safely isolated.
- Final approval of a collaboration does **not** automatically authorize integration or deployment: the `MAINTAINER` decides.

## 7. Startup checklist

- [ ] I know my maintainer-assigned agent identity.
- [ ] I read this file, then the collaboration protocol, state, and committed handoff.
- [ ] I confirmed project-specific commands and actual specification paths.
- [ ] I verified the branch and working tree, and checked that the handoff owner is me before writing.
- [ ] I understand the next permitted action and what evidence must be committed.

**Maintainer setup note:** Leave all placeholders and the other two template files **inactive** until you have supplied a concrete goal and explicitly asked Agent A to bootstrap the collaboration.
