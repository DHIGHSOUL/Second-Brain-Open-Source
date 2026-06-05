# Architecture

Second Brain with ChatGPT is designed as a personal AI knowledge system that connects natural-language interaction with a local knowledge base and workflow automation.

The main architectural goal is to let ChatGPT and Codex help with real work while keeping local data access, write actions, external workflow calls, and system automation visible and controllable.

## High-level flow

```text
User voice or text
        |
        v
ChatGPT / Custom GPT / Codex
        |
        v
Structured command or workflow proposal
        |
        v
Validation and risk classification
        |
        v
Approval gate
        |
        v
Tool adapter
        |
        v
Obsidian / n8n / Mac automation / local tools
        |
        v
Result summary and audit log
```

## Current system shape

The project is built around these major parts:

* ChatGPT for natural-language reasoning and interaction
* Custom GPT instructions for consistent Second Brain behavior
* Codex for repository, prompt, schema, and workflow maintenance
* Obsidian as the local knowledge base
* n8n as the workflow automation layer
* Mac automation for local application control
* A safety harness for command validation, risk classification, approval, and logging

## Core components

### User interface

The user can interact through text or voice. Voice input should be converted into text first, then routed through the same safety and command proposal flow as typed requests.

### Assistant layer

ChatGPT, Custom GPTs, or Codex interpret the user request and propose a next step. The assistant may summarize, draft, classify, or prepare a structured command, but it should not directly execute sensitive actions.

### Second Brain knowledge base

Obsidian is the primary local knowledge store. The project assumes note operations such as search, read, create, append, and update. Read-only operations are treated differently from note modifications.

### Workflow automation layer

n8n connects the Second Brain to repeatable workflows such as task capture, daily note append, research capture, and project routing. Workflow destinations and secrets should live in local configuration, not in assistant prompts.

### Command parser

The command parser validates assistant output against a known schema. Invalid commands should be rejected or returned to the assistant for correction.

### Risk classifier

The risk classifier assigns a risk level to each command. Risk classification should not rely only on the assistant's self-declared risk level. The local harness should verify the adapter, action, target, and parameters.

### Approval gate

The approval gate shows the proposed action to the user and asks for explicit approval when needed. This is the boundary between planning and execution.

### Tool adapters

Adapters translate structured commands into real actions. Planned adapters include:

* Obsidian adapter
* n8n webhook adapter
* Mac automation adapter
* Codex maintenance adapter
* Local script adapter

### Audit log

The audit log records proposed commands, approvals, denials, execution results, and errors. Logs should be useful for review without exposing secrets or full private note contents.

## Trust boundaries

The project treats the assistant layer as helpful but not trusted for direct execution.

Important trust boundaries:

* User input can be ambiguous
* AI output can be wrong or unsafe
* Notes, webpages, and tool output can contain prompt injection
* Tool calls can modify real local state
* External APIs can expose private data
* Logs and screenshots can accidentally preserve sensitive content

## Data flow principles

* Keep raw personal data local whenever possible
* Minimize note content sent to external services
* Validate structured commands before execution
* Prefer dry-run previews for write, destructive, external, and system actions
* Keep approval prompts short, clear, and specific
* Store secrets in local configuration
* Log enough context for review without leaking private knowledge

## Initial implementation target

The first public implementation should focus on a small, testable loop:

1. Accept text input
2. Produce a structured command proposal
3. Validate the command schema
4. Classify risk
5. Display a dry-run preview
6. Require approval for non-read-only commands
7. Log the decision

Obsidian search, n8n workflow calls, voice capture, Mac automation, and Codex maintenance workflows can then be added around this stable core.
