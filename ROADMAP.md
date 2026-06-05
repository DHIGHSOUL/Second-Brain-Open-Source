# Roadmap

This roadmap describes the planned development path for Second Brain with ChatGPT.

## Phase 1: Open-source foundation

* Rewrite the repository around the Second Brain project
* Document the current personal architecture
* Publish sanitized screenshots and diagrams
* Write the safety model
* Define the command schema
* Draft Custom GPT instructions
* Draft n8n workflow examples
* Prepare initial documentation for public readers

## Phase 2: Knowledge base workflows

* Document the Obsidian vault structure
* Define note types such as inbox, project, daily, research, and task notes
* Add safe note search examples
* Add read-only note summary examples
* Add approved note creation examples
* Add approved note update examples
* Prevent duplicate, unsafe, or unclear writes

## Phase 3: AI command layer

* Convert natural-language requests into structured command proposals
* Validate commands against the schema
* Add deterministic risk classification
* Implement dry-run previews
* Add approval prompts for write, destructive, external, and system actions
* Record decisions in a local audit log

## Phase 4: n8n automation workflows

* Document the current n8n workflow patterns
* Add examples for task capture, daily note append, and research capture
* Keep webhook URLs in local configuration
* Validate workflow payloads before execution
* Return structured workflow results
* Add examples for failure handling and retries

## Phase 5: Voice and Mac automation

* Build a basic voice command capture flow
* Convert voice input into text
* Route voice commands through the same command approval layer
* Open applications through approved Mac automation
* Add safety rules for AppleScript and shell execution
* Add rollback or recovery notes where possible

## Phase 6: Codex-assisted maintenance

* Use Codex to maintain prompts, schemas, docs, and examples
* Add tests for command schema validation
* Add tests for risk classification
* Add examples for safe repository maintenance workflows
* Document how Codex fits into the Second Brain development loop

## Future ideas

* Local dashboard for command review
* Per-tool permission profiles
* Policy-as-code guardrails
* Voice command memory
* Multi-assistant orchestration
* Local-first task inbox
* Exportable starter kit for other users
* Example vault template with fake data
