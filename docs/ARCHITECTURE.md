# Architecture

Jarvis Second Brain Harness is designed as a layered system that separates natural-language interaction from tool execution.

The main architectural goal is to make AI-generated actions inspectable before they touch local files, external APIs, or operating-system automation.

## High-level flow

```text
User voice or text
        |
        v
Intent capture
        |
        v
AI assistant layer
        |
        v
Structured command proposal
        |
        v
Risk classifier
        |
        v
Approval gate
        |
        v
Tool adapter
        |
        v
Local or external action
        |
        v
Execution log
```

## Core components

### Voice command interface

Captures a spoken user request and converts it into text. This layer should not execute commands directly. It only passes user intent to the AI assistant layer.

### AI assistant layer

Uses ChatGPT, Custom GPTs, or Codex to interpret the user request and propose a structured command. The assistant should explain what it plans to do and return machine-readable command data.

### Command parser

Validates assistant output against a known schema. Invalid commands should be rejected or returned to the assistant for correction.

### Intent classifier

Identifies the general purpose of a request, such as reading notes, creating a note, opening an application, calling an external service, or running a local command.

### Risk classifier

Assigns a risk level to each command. Risk classification is independent from the assistant's intent. The harness should not rely only on the AI model's self-declared risk level.

### Approval gate

Shows the proposed action to the user and asks for explicit approval when needed. The approval gate is the boundary between planning and execution.

### Tool adapters

Adapters translate structured commands into real actions. Planned adapters include:

* Mac automation adapter
* Obsidian MCP adapter
* n8n webhook adapter
* Codex command harness
* Local script adapter

### Execution log

Records proposed commands, approvals, denials, execution results, and errors. Logs should be reviewable without exposing secrets.

## Trust boundaries

The project treats the assistant layer as untrusted for execution purposes. The assistant may propose useful actions, but the harness must validate, classify, and gate those actions.

Important trust boundaries:

* User input can be ambiguous
* AI output can be wrong or unsafe
* Tool calls can modify real local state
* External APIs can expose private data
* Logs can accidentally preserve sensitive content

## Data flow principles

* Keep raw user data local whenever possible
* Minimize personal data sent to external APIs
* Validate structured commands before execution
* Prefer dry runs for write, destructive, external, and system actions
* Keep approval prompts short, clear, and specific
* Log enough context for review without leaking secrets

## Initial implementation target

The first implementation should focus on a small, testable loop:

1. Accept text input
2. Produce a structured command proposal
3. Validate the command schema
4. Classify risk
5. Display a dry-run preview
6. Require approval for non-read-only commands
7. Log the decision

Voice control, Mac automation, Obsidian MCP, and n8n integrations can then be added as adapters around this stable harness.
