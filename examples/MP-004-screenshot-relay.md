# MP-004 — Screenshot Relay

## Situation

A screen state, error, or UI result is captured and uploaded to convey it to another system.

## Current Flow

```text
Application state
  ↓ screenshot
Human
  ↓ upload
Destination system
```

## What the Human Decides

Which state matters, where to crop, and whether sensitive information needs redaction.

## What the Human Transports

The display state, when the capture is only a mechanical stand-in for a shareable reference or structured error.

## Why It Exists

The source does not expose shareable state, logs, or a contextual handoff to the destination.

## Possible Improvement

Offer a consented “share current state” action that can attach structured diagnostics, a deep link, or a redacted visual preview.

## Status

`Common`

## Boundary

Screenshots used to explain, annotate, select, or redact intentionally are not automatically Meat Protocol. The candidate is the purely transport-oriented portion.
