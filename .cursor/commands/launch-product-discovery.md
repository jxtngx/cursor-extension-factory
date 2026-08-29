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

### Q4 — Rust crate (required)

This factory is TS + Rust like Ruff. Ask:

- Crate name(s) under `crates/`
- What the binary **does** (lint, format, analyze, …) — this is the product
- Interface: argv/stdin JSON (CLI) vs LSP methods (if sidecar is lsp)
- Target triples to **bundle** in the VSIX (name them; default: current desktop + document others)
- Non-goal: reimplementing that logic in TypeScript

### Q5 — Contributions

List `contributes` keys they actually need (commands, configuration, views, languages, themes, debuggers, notebooks, menus). Non-goals: at least three contribution points they will **not** add.

### Q6 — Cursor coexistence

```
Does this inject completions, inline chat, or keybindings that can fight Cursor's AI?
- No
- Yes — document disable/conflict behavior
```

### Q7 — Secrets and network

```
- None
- Talks to a local process (LSP/DAP)
- Talks to a cloud API (name it; tokens in SecretStorage, not settings)
```

### Q8 — Test matrix

```
Must pass in:
- Cursor desktop (required)
- VS Code desktop (required if dual-publish)
```

### Q9 — Publish

Confirm Open VSX. If dual, same extension ID. Homepage must be a **real domain** if they want Cursor verification (GitHub README is not enough per Cursor help).

### Q10 — Repo

- GitHub `owner/repo` for the generated product
- Sprint plan filename `<slug>-sprint.plan.md`

## Write the spec

Use [technical-requirements-template.md](../templates/technical-requirements-template.md) and [extension-spec-template.md](../templates/extension-spec-template.md).

Do not implement.
Hand back to `@init-extension` step 3 if invoked from there.
