# Security Policy

Second Brain with ChatGPT connects AI assistants with personal notes, local tools, n8n workflows, Mac automation, API calls, and Obsidian-related integrations. Because the system touches private knowledge and local automation, security is a core part of the project design.

## Security goals

The main goal is to prevent AI-generated commands from causing unintended data loss, privacy leaks, or unsafe local automation.

This project is designed around the following principles:

* No destructive command should run without explicit user approval
* Commands should be classified by risk before execution
* File deletion, overwrite, external transmission, and system-level actions should require confirmation
* The system should clearly explain what it is about to do
* Users should be able to review logs and command history
* API keys, local credentials, and private vault contents must never be exposed in prompts, logs, screenshots, or public examples

## Command risk levels

Commands should be classified into the following categories:

* `read_only`: Search notes, read files, summarize local context, inspect status
* `write`: Create notes, update files, append tasks, modify local data
* `destructive`: Delete files, overwrite content, remove records, reset workflows
* `external`: Send data to external APIs, webhooks, or network services
* `system`: Open applications, run scripts, control macOS, execute shell commands

## Approval model

The default execution model should be:

1. Understand the user request
2. Convert the request into a structured command
3. Validate the command schema
4. Classify the command risk level
5. Show the planned action to the user
6. Ask for approval when needed
7. Execute only after approval
8. Log the result with sensitive data redacted

## Sensitive areas

The following areas require special care:

* Obsidian vault reads and writes
* Personal note summaries
* Mac automation
* Shell command execution
* MCP tool access
* n8n webhook calls
* REST API authentication
* Network requests
* File deletion or overwrite
* Personal knowledge data export

## Handling secrets

Secrets should be stored outside the repository and outside prompt examples.

Do not commit:

* API keys
* OAuth tokens
* n8n webhook URLs
* Local file paths containing private user information
* Obsidian vault contents
* Voice transcripts containing sensitive data
* Command logs containing private notes or credentials
* Screenshots containing private notes, tokens, or personal data

Use placeholder values in examples, such as `OPENAI_API_KEY`, `OBSIDIAN_VAULT_PATH`, or `N8N_WEBHOOK_URL`.

## Public example policy

Public examples should use fake note names, fake workflow names, and fake payloads. Screenshots should be reviewed before publication to make sure they do not reveal private vault content, credentials, or personal identifiers.

## Responsible disclosure

This project is currently experimental. If you find a security issue, please open a GitHub issue with a minimal reproduction or contact the maintainer privately if the issue involves sensitive information.

Please avoid posting secrets, private logs, personal vault content, private screenshots, or exploit payloads in public issues.
