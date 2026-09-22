# MP-003 — ZIP Shuttle

## Situation

A set of files or an entire project is compressed and uploaded solely so another system can inspect it.

## Current Flow

```text
Project directory
  ↓ create archive
Human
  ↓ upload ZIP
LLM or external system
```

## What the Human Decides

Which project or subset should be shared, including any confidential material that must stay out.

## What the Human Transports

The selected directory tree, repackaged as an archive.

## Why It Exists

The destination lacks scoped access to the existing project storage, or has no way to receive an intentional folder reference.

## Possible Improvement

Provide scoped repository/storage access with a selectable folder handoff, manifest preview, and explicit consent.

## Status

`Common`
