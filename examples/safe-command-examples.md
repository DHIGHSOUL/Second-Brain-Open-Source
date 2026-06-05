# Safe Command Examples

These examples show how the harness should represent common commands before execution.

## Read-only note search

```json
{
  "command_id": "cmd_search_second_brain",
  "version": "0.1",
  "intent": "search_notes",
  "risk_level": "read_only",
  "adapter": "obsidian",
  "action": "search_notes",
  "parameters": {
    "query": "Second Brain ChatGPT workflow",
    "limit": 10
  },
  "summary": "Search the Obsidian vault for notes about Second Brain ChatGPT workflows.",
  "requires_approval": false,
  "dry_run": true
}
```

Expected behavior: the harness may execute this without approval if the Obsidian adapter is configured as read-only.

## Create a note

```json
{
  "command_id": "cmd_create_codex_note",
  "version": "0.1",
  "intent": "create_note",
  "risk_level": "write",
  "adapter": "obsidian",
  "action": "create_note",
  "parameters": {
    "path": "Projects/Jarvis/Codex Harness.md",
    "content": "# Codex Harness\n\nDraft notes for the local command harness."
  },
  "summary": "Create a new Obsidian note for Codex Harness planning.",
  "requires_approval": true,
  "approval_reason": "This will create a new file in the Obsidian vault.",
  "dry_run": true
}
```

Expected behavior: show a preview and require approval before writing the file.

## Update a note

```json
{
  "command_id": "cmd_update_project_index",
  "version": "0.1",
  "intent": "update_note",
  "risk_level": "write",
  "adapter": "obsidian",
  "action": "append_to_note",
  "parameters": {
    "path": "Projects/Jarvis/Index.md",
    "content": "- Add command schema validation to the prototype."
  },
  "summary": "Append a task to the Jarvis project index note.",
  "requires_approval": true,
  "approval_reason": "This will modify an existing note.",
  "dry_run": true
}
```

Expected behavior: show the exact note path and appended content before approval.

## Delete a note

```json
{
  "command_id": "cmd_delete_old_note",
  "version": "0.1",
  "intent": "delete_note",
  "risk_level": "destructive",
  "adapter": "obsidian",
  "action": "delete_note",
  "parameters": {
    "path": "Projects/Jarvis/Old Draft.md"
  },
  "summary": "Delete the old Jarvis draft note.",
  "requires_approval": true,
  "approval_reason": "This will remove a note from the Obsidian vault.",
  "dry_run": true
}
```

Expected behavior: require explicit confirmation. The harness should prefer moving the file to trash or making a backup over permanent deletion.

## Open an application

```json
{
  "command_id": "cmd_open_obsidian",
  "version": "0.1",
  "intent": "open_application",
  "risk_level": "system",
  "adapter": "mac_automation",
  "action": "open_application",
  "parameters": {
    "application": "Obsidian"
  },
  "summary": "Open the Obsidian application on macOS.",
  "requires_approval": true,
  "approval_reason": "This will control a local macOS application.",
  "dry_run": true
}
```

Expected behavior: require approval unless the user has explicitly allowlisted this exact action.

## Call an n8n webhook

```json
{
  "command_id": "cmd_send_task_to_n8n",
  "version": "0.1",
  "intent": "call_webhook",
  "risk_level": "external",
  "adapter": "n8n",
  "action": "call_webhook",
  "parameters": {
    "webhook_name": "task_inbox",
    "payload_preview": {
      "title": "Review Jarvis command schema",
      "source": "jarvis_harness"
    }
  },
  "summary": "Send a new task to the configured n8n task inbox webhook.",
  "requires_approval": true,
  "approval_reason": "This will send task data to an external workflow endpoint.",
  "dry_run": true
}
```

Expected behavior: show the destination and payload preview before approval. The real webhook URL should come from local configuration, not the assistant response.
