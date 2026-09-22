# MP-002 — File Path Relay

## Situation

A person has found a file in an editor or file browser and an agent can access the same repository, but the person still copies and pastes the path into chat.

## Current Flow

```text
Editor file tree
  ↓ find player.rs
Human
  ↓ Copy Relative Path / paste into chat
Agent with repository access
```

## What the Human Decides

That `player.rs` is the file the agent should inspect.

## What the Human Transports

The reference information: its path or identifier.

## Why It Exists

The editor selection and agent context are separate interfaces, even when both point at the same workspace.

## Possible Improvement

Add an explicit, permission-aware action such as **Send reference to agent**. The tool carries a stable reference; the person retains file selection.

## Status

`Common`

## Comic

![Four-panel comic: a developer carries a player.rs label to an AI that already has repository access.](../comics/MP-002-file-path-relay.png)

Generated with OpenAI image generation; this comic is an illustrative depiction, not a product UI.
