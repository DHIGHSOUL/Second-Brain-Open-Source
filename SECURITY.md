# Security Policy

This project connects AI assistants with local tools, voice commands, Mac automation, API calls, and Obsidian MCP. Because of this, security is a core part of the project design.

## Security goals

The main goal is to prevent AI-generated commands from causing unintended or destructive actions.

This project is designed around the following principles:

* No destructive command should run without explicit user approval
* Commands should be classified by risk before execution
* File deletion, overwrite, external transmission, and system-level actions should require confirmation
* The system should clearly explain what it is about to do
* Users should be able to review logs and command history
* API keys and local credentials must never be exposed in prompts, logs, or public examples

## Command risk levels

Commands should be classified into the following categories:

* `read_only`: Search files, read notes, inspect status
* `write`: Create notes, update files, modify local data
* `destructive`: Delete files, overwrite content, remove records
* `external`: Send data to external APIs or network services
* `system`: Open applications, run scripts, control the operating system

## Approval model

The default execution model should be:

1. Understand the user request
2. Convert the request into a structured command
3. Classify the command risk level
4. Show the planned action to the user
5. Ask for approval when needed
6. Execute only after approval
7. Log the result

## Sensitive areas

The following areas require special care:

* Mac automation
* Shell command execution
* Obsidian vault modification
* MCP tool access
* REST API authentication
* Network requests
* File deletion or overwrite
* Personal knowledge data export

## Handling secrets

Secrets should be stored outside the repository and outside prompt examples.

Do not commit:

* API keys
* OAuth tokens
* Local file paths containing private user information
* Obsidian vault contents
* Voice transcripts containing sensitive data
* Command logs containing private notes or credentials

Use placeholder values in examples, such as `OPENAI_API_KEY`, `OBSIDIAN_VAULT_PATH`, or `N8N_WEBHOOK_URL`.

## Responsible disclosure

This project is currently experimental. If you find a security issue, please open a GitHub issue with a minimal reproduction or contact the maintainer privately if the issue involves sensitive information.

Please avoid posting secrets, private logs, personal vault content, or exploit payloads in public issues.
