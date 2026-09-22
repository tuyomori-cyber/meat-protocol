# Meat Protocol

> **Human as selector, not transporter.**

**Meat Protocol** is a small, serious-with-a-straight-face catalog of moments when a human manually carries information between systems even though the systems could plausibly carry it themselves.

People should decide *what* deserves attention. They should not have to act as the transport layer just to move a file reference, a block of context, or an AI response to the next place.

## The idea in one picture

```text
Before
Human = selector + transporter

After
Human = selector
Tool  = transporter
```

This is not an argument against human-in-the-loop workflows or careful review. The question is narrower:

> Is the person making a decision here, or merely moving already-selected information?

If it is the latter and a system could do it safely, it may be a Meat Protocol.

## Start here

- [Definition](docs/definition.md) — scope, terminology, and the test for a candidate.
- [Principles](docs/principles.md) — the project’s point of view and non-goals.
- [Protocol catalog](examples/README.md) — the initial six examples.
- [Reference implementations](implementations/README.md) — practical ways to reduce a relay.
- [Contributing](CONTRIBUTING.md) — add a case without turning the catalog into a complaint board.

## Representative relays

| ID | Name | The unnecessary journey |
| --- | --- | --- |
| [MP-001](examples/MP-001-clipboard-relay.md) | Clipboard Relay | copy an output, then paste it into another system |
| [MP-002](examples/MP-002-file-path-relay.md) | File Path Relay | discover a file, then manually tell an agent its path |
| [MP-003](examples/MP-003-zip-shuttle.md) | ZIP Shuttle | package a project just to hand it to a system |
| [MP-004](examples/MP-004-screenshot-relay.md) | Screenshot Relay | capture a state solely to transfer it elsewhere |
| [MP-005](examples/MP-005-ai-to-ai-relay.md) | AI-to-AI Relay | shuttle unedited responses back and forth between agents |
| [MP-006](examples/MP-006-re-explanation-protocol.md) | Re-explanation Protocol | reconstruct established context in a new session |

## A comic, because this should feel slightly absurd

![A person carries a file path from an editor to an AI that already has repository access.](comics/MP-002-file-path-relay.png)

*MP-002 — the machine can read the repository; the human still has to carry the pointer.*

## Status vocabulary

`Common` · `Workaround available` · `Partially automated` · `Solved` · `Historical`

“Solved” and “Historical” entries remain useful: they document a UX problem and how it was removed.

## License

The repository content is available under the [MIT License](LICENSE).
