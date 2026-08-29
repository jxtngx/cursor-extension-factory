# Launch Product Discovery (extension factory)

Same *shape* as the other factories: questionnaire → technical requirements → architect → scrum.
Questions are about a **VS Code extension for Cursor**, not a Swift app or a LangChain graph.

Called from `@init-extension` after kind is locked. If `TRACK.md` is missing, run `@init-extension` instead.

## Question sequence

### Q1 — Job

- One-sentence: what the extension *does* in the editor
- Problem statement (why a settings.json snippet is not enough)
- Display name + proposed `publisher.extension` id (they supply publisher; do not invent)

### Q2 — Users

```
Who is the primary user?
- Individual
- Team (Cursor enterprise / allowlists matter)
- Marketplace (Open VSX public)
```

### Q3 — Activation

```
When does it activate?
- onCommand
- onLanguage (name languages)
- onView
- onStartupFinished (justify)
- other (name the event)
```

Prefer lazy activation.

### Q4 — Contributions

List `contributes` keys they actually need (commands, configuration, views, languages, themes, debuggers, notebooks, menus). Non-goals: at least three contribution points they will **not** add.

### Q5 — Cursor coexistence

```
Does this inject completions, inline chat, or keybindings that can fight Cursor's AI?
- No
- Yes — document disable/conflict behavior
```

### Q6 — Secrets and network

```
- None
- Talks to a local process (LSP/DAP)
- Talks to a cloud API (name it; tokens in SecretStorage, not settings)
```

### Q7 — Test matrix

```
Must pass in:
- Cursor desktop (required)
- VS Code desktop (required if dual-publish)
```

### Q8 — Publish

Confirm Open VSX. If dual, same extension ID. Homepage must be a **real domain** if they want Cursor verification (GitHub README is not enough per Cursor help).

### Q9 — Repo

- GitHub `owner/repo` for the generated product
- Sprint plan filename `<slug>-sprint.plan.md`

## Write the spec

Use [technical-requirements-template.md](../templates/technical-requirements-template.md) and [extension-spec-template.md](../templates/extension-spec-template.md).

Do not implement.
Hand back to `@init-extension` step 3 if invoked from there.
