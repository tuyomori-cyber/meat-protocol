# Meat-to-Auto Reference Implementations

Implementations show how a human relay was actually reduced. They need not automate human judgment: a good implementation preserves intentional choices and automates the separable mechanical handoff.

## Primitive IO

Primitive IO is an example of this approach. It presents a Dropbox file tree inside ChatGPT, lets a person select a Markdown or text file, and passes the selected file reference to chat input. The official Dropbox integration can then read the file.

```text
Before
Dropbox → human searches → copy path → return to chat → paste

After
Dropbox tree in chat → human selects → Primitive IO transports reference → chat
```

### Human roles

- **Selection:** choose the source and file.
- **Transport:** pass its reference into chat.

### Mechanical Human Work and automation

Copying the path and changing applications are mechanical work. Primitive IO automates that handoff while preserving human selection.

```text
Human       = selector
Primitive IO = transporter
```

An earlier workflow also included asking ChatGPT to format a discussion as Markdown, selecting all, copying, opening a local editor, creating a file, pasting, and saving it. Where the destination project becomes a stable rule, much of that sequence can similarly become mechanical human work; choosing the correct project can still require routing judgment.

Future APIs, plugins, MCP servers, or shared-storage integrations may solve the same handoff differently. Meat-to-Auto records both the automation and the human roles intentionally preserved.
