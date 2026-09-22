# MP-005 — AI-to-AI Relay

## Situation

A person copies an unedited answer from AI A to AI B, then carries AI B’s answer back to AI A.

## Current Flow

```text
AI A
  ↓
Human
  ↓
AI B
  ↓
Human
  ↓
AI A
```

## What the Human Decides

Whether to consult another model, what context it should receive, and whether to accept either answer.

## What the Human Transports

The response payload during an otherwise unedited round trip.

## Why It Exists

Agent sessions are isolated and lack a user-controlled exchange channel with provenance and boundaries.

## Possible Improvement

Use a user-initiated relay that previews exactly what crosses systems, identifies its source, and keeps the human approval point.

## Status

`Common`
