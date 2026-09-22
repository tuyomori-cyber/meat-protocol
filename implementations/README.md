# Reference Implementations

An implementation does not need to automate a person’s judgment to reduce a Meat Protocol. A good implementation preserves a visible selection and automates the mechanical handoff that follows.

## Primitive IO

Primitive IO is an example of this approach. It presents a Dropbox file tree inside ChatGPT, lets a person select a Markdown or text file, and passes the selected file reference to the chat input. The official Dropbox integration can then read the file.

```text
Before
Dropbox → human searches → copy path → return to chat → paste

Primitive IO
Dropbox tree in chat → human selects → Primitive IO transports reference → chat
```

Primitive IO is not a requirement or the sole solution. It illustrates the division of labor:

```text
Human       = selector
Primitive IO = transporter
```

Future APIs, plugins, MCP servers, or shared-storage integrations may solve the same handoff differently.
