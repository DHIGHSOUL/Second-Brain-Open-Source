# Harness Design

The harness is the safety and orchestration layer between an AI assistant and the user's working environment.

It receives structured command proposals, validates them, classifies risk, asks for approval when required, executes approved commands through adapters, and records the result.

## Design goals

* Separate planning from execution
* Use structured commands instead of free-form shell text
* Make risk classification explicit
* Require approval for sensitive actions
* Support dry-run previews
* Keep adapter behavior narrow and auditable
* Make logs useful for debugging and review

## Non-goals

* The harness should not be a general-purpose unsupervised agent
* The harness should not silently execute destructive commands
* The harness should not expose secrets to prompts or logs
* The harness should not bypass operating-system permission prompts

## Command lifecycle

```text
proposed -> validated -> classified -> approved or denied -> executed -> logged
```

### 1. Proposed

The assistant returns a structured command proposal. The proposal should include:

* Command ID
* Intent
* Target adapter
* Action
* Parameters
* Expected risk level
* Human-readable summary

### 2. Validated

The harness validates the proposal against the command schema. Required fields must be present and parameter types must match the expected shape.

### 3. Classified

The harness classifies risk using deterministic rules and, optionally, model-assisted review.

Examples:

* Searching notes is usually `read_only`
* Creating a new note is usually `write`
* Deleting a note is `destructive`
* Sending note contents to an API is `external`
* Running AppleScript or shell commands is `system`

### 4. Approved or denied

The approval gate decides whether the command can run.

Suggested defaults:

* `read_only`: may run without approval if the adapter is trusted
* `write`: requires approval unless the user configured a safe allowlist
* `destructive`: always requires approval
* `external`: always requires approval when user data is included
* `system`: always requires approval unless narrowly allowlisted

### 5. Executed

The adapter executes the command. Adapters should return structured results instead of raw logs when possible.

### 6. Logged

The harness records:

* Timestamp
* Command ID
* User request summary
* Adapter and action
* Risk level
* Approval decision
* Execution status
* Redacted result summary

## Adapter interface

Each adapter should expose a small set of named actions.

Example adapter shape:

```json
{
  "adapter": "obsidian",
  "actions": [
    "search_notes",
    "read_note",
    "create_note",
    "update_note"
  ]
}
```

Adapters should avoid accepting arbitrary shell commands. If shell access is needed, the shell adapter should be isolated and heavily restricted.

## Dry-run behavior

Dry-run mode should show what would happen without changing local state.

For write actions, the dry run should display:

* Target file or note
* Whether the target already exists
* Proposed content summary
* Expected modification type

For destructive actions, the dry run should display:

* Exact target
* Data that would be removed
* Whether recovery is possible
* Confirmation phrase required

## Approval prompt format

Approval prompts should be short and concrete.

Example:

```text
Jarvis wants to create a new Obsidian note.

Target: Projects/Jarvis/Ideas.md
Risk: write
Reason: This will add a new file to your vault.

Approve? yes/no
```

## Recommended implementation order

1. Implement schema validation
2. Implement deterministic risk rules
3. Implement dry-run previews
4. Implement approval prompts
5. Implement read-only Obsidian actions
6. Add write actions with approval
7. Add Mac automation actions with approval
8. Add external workflow calls through n8n
