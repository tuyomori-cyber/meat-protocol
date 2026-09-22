# Meat Protocol — Initial Specification

## 1. Overview

**Meat Protocol** is a half-joking, half-serious catalog of cases where people manually carry references, state, or context between systems even though systems could plausibly exchange it themselves.

LLMs and agents increasingly access file systems, cloud storage, and repositories. Still, people copy and paste, transcribe paths, create ZIP files, and re-explain context merely to tell an AI what to receive. This project calls those cases **Meat Protocol**.

> **Human as selector, not transporter.**

People should decide what to pass along; where possible, machines should perform the following mechanical transport.

## 2. Goals and non-goals

- Make manual information transport visible as a UX problem.
- Organize product-specific frustrations under a shared concept.
- Catalog cases and possible improvements, using humor without technical grandiosity.
- Do not reject human-in-the-loop work, delegate every decision to AI, prescribe one vendor or protocol, or pursue complete automation.

## 3. Core concepts

### Human as selector

People select a target: “Look at this specification,” “Compare these three files,” or “Show this error to another AI.” This is valuable intent and judgment.

### Human as transporter

After selection, people mechanically copy a path, move to another chat, paste it, and request that the file be read. This is the project’s main subject.

Ask: **Is the person deciding here, or simply carrying an already-decided payload?** The latter is a candidate when a system could safely substitute.

## 4. Representative cases

- **MP-001 Clipboard Relay:** copy output from one system and paste it into another.
- **MP-002 File Path Relay:** find a file in a GUI, then copy its path into an agent that already accesses the same repository.
- **MP-003 ZIP Shuttle:** archive and upload a project solely for another system to inspect.
- **MP-004 Screenshot Relay:** capture and upload a state or error; distinguish mechanical transfer from intentional selection or redaction.
- **MP-005 AI-to-AI Relay:** carry unedited answers between AI A and AI B; human review and editing are not the relay.
- **MP-006 Re-explanation Protocol:** reconstruct settled background, rationale, and constraints after switching systems or sessions.

## 5. Primitive IO

Primitive IO is an example implementation. It shows a Dropbox tree in ChatGPT, lets a person select a Markdown or text file, and passes its reference to chat input; the official Dropbox integration reads it.

```text
Before: Human = selector + transporter
After:  Human = selector; Primitive IO = transporter
```

It automates only transport of a selected reference, not retrieval of file contents. The target-selection UX can remain relevant even as APIs, MCP, and plugins evolve.

## 6. MCP

MCP principally connects `LLM <-> External System`; Meat Protocol also concerns the target-selection UX of `Human <-> LLM`. Filesystem access alone does not answer: “How can a person easily point to this file they already found?” These are complementary layers.

## 7. Repository and catalog

The tentative repository is `meat-protocol`, with README, LICENSE, `docs/`, `examples/`, and `comics/`; a GitHub Wiki may mirror or expand it. Entries use a consistent shape: Situation, Current Flow, What the Human Decides, What the Human Transports, Why It Exists, Possible Improvement, Status, and Comic. Statuses are `Common`, `Workaround available`, `Partially automated`, `Solved`, and `Historical`.

## 8. Comics and tone

Comics make the awkward relay visible at a glance and should stand alone when shared. The project should be neither a pure joke nor a rigid standard: the name is silly, the observation serious, and the aim is not to attack people or products. The humor is that remarkably capable systems can still require a person to carry a pointer across the room.

## 9. Initial MVP and future work

Keep the first release lightweight and Markdown-centered: define the concept in README, state the principle, add MP-001 through MP-006, publish at least one comic, include Primitive IO as an example, and provide a template for new cases. Future work includes research into prior terminology, stricter criteria and status rules, Wiki versus repository source-of-truth, contribution flows, licenses, comic disclosures, additional implementations, and preserving solved relays as UX history.

## 10. Direction for Codex

Create the initial GitHub structure without a website or complex application. Make the README self-contained, support later Wiki expansion, retain the humor, distinguish judgment from transport, and preserve later-solved cases as UX history. This is an initial proposal that may evolve through discussion and implementation.
