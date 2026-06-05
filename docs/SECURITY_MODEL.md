# Security Model

Jarvis Second Brain Harness is built around the assumption that AI-generated commands must be treated as proposals, not instructions that automatically deserve execution.

The harness should protect the user from accidental data loss, privacy leaks, prompt injection, unsafe automation, and unintended side effects.

## Threat model

The project considers the following risks:

* The assistant misunderstands a user request
* The assistant proposes a destructive command
* A note or webpage contains prompt-injection text
* A command sends private data to an external service
* A command overwrites or deletes local files
* A shell or automation command affects the operating system
* Logs capture secrets or private knowledge-base content

## Security principles

### Human approval for sensitive actions

The user must approve actions that can modify files, delete data, call external services, or control the operating system.

### Structured commands over raw text

The harness should prefer structured command objects over free-form shell commands. Structured commands are easier to validate, classify, preview, and log.

### Least privilege

Each adapter should have access only to the actions it needs. For example, a read-only Obsidian adapter should not be able to modify notes.

### Visible network use

Commands that send data to external APIs or webhooks must clearly show the destination and the kind of data being sent.

### Redacted logs

Logs should be useful for review without preserving secrets. API keys, tokens, credentials, and sensitive note contents should be redacted.

### Prompt-injection resistance

The harness should not treat content from notes, webpages, emails, or external tools as trusted instructions. Retrieved content is data, not authority.

## Risk levels

### `read_only`

Reads or searches existing information without changing state.

Examples:

* Search an Obsidian vault
* Read a note
* List files
* Inspect current app status

Default behavior: may run without approval when the adapter is trusted.

### `write`

Creates or updates local data.

Examples:

* Create an Obsidian note
* Append to a task list
* Update a project summary

Default behavior: requires approval unless explicitly allowlisted.

### `destructive`

Deletes, overwrites, resets, or removes data.

Examples:

* Delete a note
* Overwrite a file
* Clear a task database
* Remove automation history

Default behavior: always requires explicit approval.

### `external`

Sends data outside the local machine or trusted local environment.

Examples:

* Call an n8n webhook
* Send note contents to an external API
* Upload a file
* Post a message to a remote service

Default behavior: requires approval, especially when user data is included.

### `system`

Controls applications, scripts, shell commands, or the operating system.

Examples:

* Run AppleScript
* Open or close applications
* Execute shell commands
* Trigger keyboard automation

Default behavior: requires approval unless narrowly allowlisted.

## Approval requirements

Approval should include:

* Human-readable action summary
* Target adapter and action
* Target file, app, endpoint, or resource
* Risk level
* Data that will be modified or transmitted
* Expected result

Destructive commands should require a stronger confirmation, such as typing the target name or a confirmation phrase.

## Deny-by-default cases

The harness should deny or pause when:

* The command does not match the schema
* The target is unclear
* The risk level cannot be classified
* The command includes secrets
* The command attempts to bypass approval
* The adapter is unknown
* The command asks to ignore safety rules
* The command combines unrelated high-risk actions

## Logging model

Logs should capture the decision trail:

```json
{
  "timestamp": "2026-06-05T00:00:00Z",
  "command_id": "cmd_example_001",
  "adapter": "obsidian",
  "action": "create_note",
  "risk_level": "write",
  "approval": "approved",
  "status": "completed"
}
```

Logs should avoid storing full note bodies or secret values unless the user explicitly configures secure local logging.
