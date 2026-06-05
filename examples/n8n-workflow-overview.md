# n8n Workflow Overview

This document describes example n8n workflows that can be connected to Second Brain with ChatGPT.

The goal is to use n8n as an automation bridge while keeping the local harness responsible for validation, risk classification, approval, and logging.

## Design principles

* The harness decides whether a workflow can be called
* n8n webhooks should not be exposed directly to the assistant
* Webhook URLs should live in local configuration
* Payloads should be small and explicit
* Sensitive data should be redacted or omitted
* Workflow results should return structured status data

## Example workflow: task inbox

Purpose: add a task to a trusted task system.

Suggested flow:

```text
Approved Second Brain command
        |
        v
n8n webhook: task_inbox
        |
        v
Validate payload
        |
        v
Create task
        |
        v
Return task ID and status
```

Example payload:

```json
{
  "title": "Review Second Brain command schema",
  "source": "second_brain",
  "priority": "normal"
}
```

## Example workflow: daily note append

Purpose: append a short entry to a daily note through a controlled workflow.

Suggested flow:

```text
Approved Second Brain command
        |
        v
n8n webhook: daily_note_append
        |
        v
Validate date and content
        |
        v
Append entry
        |
        v
Return updated note path
```

Example payload:

```json
{
  "date": "2026-06-05",
  "entry": "Reviewed the initial Second Brain open-source roadmap."
}
```

## Example workflow: research capture

Purpose: capture a link or short summary into a research inbox.

Suggested flow:

```text
Approved Second Brain command
        |
        v
n8n webhook: research_capture
        |
        v
Validate URL and summary
        |
        v
Create inbox item
        |
        v
Return inbox item ID
```

Example payload:

```json
{
  "url": "https://example.com/article",
  "summary": "A short user-approved summary of the resource.",
  "tags": [
    "ai",
    "second-brain"
  ]
}
```

## Security notes

* Do not put webhook URLs in Custom GPT instructions
* Do not let the assistant choose arbitrary webhook destinations
* Require approval before sending note content outside the local environment
* Log the workflow name, not the secret webhook URL
* Prefer payload previews over raw private content in approval prompts
* Return minimal results to the assistant layer

## Suggested workflow response

n8n should return a small structured response:

```json
{
  "status": "completed",
  "workflow": "task_inbox",
  "result_id": "task_123",
  "message": "Task created."
}
```
