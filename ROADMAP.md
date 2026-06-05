# Roadmap

This roadmap describes the planned development path for Jarvis Second Brain Harness.

## Phase 1: Documentation and design

* Define the project purpose
* Document the overall architecture
* Write the safety model
* Define the command schema
* Draft Custom GPT instructions
* Draft Codex harness patterns
* Prepare initial examples

## Phase 2: Local prototype

* Build a basic voice command capture flow
* Convert voice input into text
* Send structured commands to the AI layer
* Return proposed actions instead of executing immediately
* Implement a dry-run mode
* Add command risk classification

## Phase 3: Second Brain integration

* Connect with Obsidian MCP
* Search notes
* Read notes
* Create new notes with approval
* Update existing notes with approval
* Prevent duplicate or unsafe writes

## Phase 4: Mac automation

* Open applications
* Trigger safe local actions
* Add approval for system-level commands
* Add logging
* Add rollback or recovery notes where possible

## Phase 5: Open-source release

* Improve documentation
* Add example workflows
* Add contribution guidelines
* Add issue templates
* Publish reusable prompt and harness patterns
* Collect feedback from other users

## Future ideas

* Local-first task inbox
* Voice command memory
* Multi-assistant orchestration
* Per-tool permission profiles
* Policy-as-code guardrails
* Local dashboard for command review
* Tests for command parsing and risk classification
