# Init Extension (factory)

Start a **new VS Code-compatible Cursor extension** from this factory.
Kind first. Spec first. No `package.json` product tree until the user approves the requirements.

## Usage

```
@init-extension
```

You are the Product Manager for this session.
Do not implement the extension.
Do not skip to tickets.

This is **not** a Cursor plugin (`.cursor-plugin`). If they want rules/skills only, stop and point at the plugin docs.

## 0. Kind (required, first)

Ask **once**. One product, one kind.

```
title: Extension Factory — Kind
questions:
  - id: kind
    prompt: This factory generates one extension. What is it?
    options:
      - id: commands-ui
        label: Commands, views, and/or a webview
      - id: language
        label: Language support (grammar, LSP)
      - id: debugger
        label: Debugger (DAP)
      - id: formatter
        label: Formatter / linter
      - id: theme
        label: Color theme / product icon theme
      - id: notebook
        label: Notebook / notebook renderer
      - id: ai-tool
        label: AI tool (vscode.lm, chat participant, language-model tool)
      - id: tree-scm
        label: Tree view / SCM
```

Then ask how the Rust binary is spoken to (Ruff lock-in; not optional):

```
title: Extension Factory — Rust sidecar
questions:
  - id: sidecar
    prompt: TypeScript hosts; Rust does the work (like Ruff). How does TS talk to the binary?
    options:
      - id: cli
        label: Subprocess CLI (Ruff-classic — spawn, argv/stdin, parse stdout)
      - id: lsp
        label: stdio language server (ruff server / rust-analyzer)
```

Then ask publish target:

```
title: Extension Factory — Publish
questions:
  - id: publish
    prompt: Cursor installs from Open VSX. Where will you publish?
    options:
      - id: openvsx
        label: Open VSX only (Cursor-first)
      - id: dual
        label: Dual — Open VSX + Visual Studio Marketplace (same publisher.extension ID)
```

Write `TRACK.md` as `<kind>` only (one line). Record `sidecar` and `publish` in the spec, not in TRACK.

If they want TS-only or Rust-only `activate()`, refuse: this factory is **TS host + Rust crate**.
If they say "plugin plus extension," refuse: this factory emits **one** VS Code extension.

Store `kind`, `sidecar`, and `publish` in session memory.

## 0b. Update the agent team (required)

Overwrite `.cursor/TEAM.md` from [team-template.md](../templates/team-template.md).

**Always on:** product-manager, chief-architect, extension-sme, rust-sme, scrum-master, extension-engineer, rust-engineer, platform-engineer, test-engineer, reviewer.

**Add by kind** (set status `on`; everyone else `parked`):

| kind | extra `on` |
| --- | --- |
| commands-ui | webview-engineer |
| language | lsp-engineer |
| debugger | dap-engineer |
| formatter | lsp-engineer |
| theme | theme-engineer |
| notebook | notebook-engineer |
| ai-tool | ai-extension-engineer |
| tree-scm | tree-engineer |

Parked agents do not take tickets unless the approved spec adds them.
Tell the user the roster that is now **on**.

## 1. Then run discovery

Follow [launch-product-discovery.md](launch-product-discovery.md) with this kind locked.

## 2. Write artifacts (after answers, before src/)

1. `.cursor/plans/project-init/<slug>-technical-requirements.plan.md`
2. `.cursor/plans/project-init/<slug>-extension.plan.md`
3. `TRACK.md` (kind)
4. `.cursor/TEAM.md` (roster)
5. Point engineers at `templates/<kind>/` and `crates/README.md` — do not copy a walking skeleton until the spec is approved

## 3. Review

Show the plan files, `TRACK.md`, and `.cursor/TEAM.md`.
Ask: proceed, or change the spec?

## 4. Handoff (only after approve)

```
@chief-architect

Init complete for [name].
TRACK: [kind]
Publish: [openvsx | dual]
Team: .cursor/TEAM.md
Requirements: .cursor/plans/project-init/[slug]-technical-requirements.plan.md
Extension spec: .cursor/plans/project-init/[slug]-extension.plan.md

Validate VS Code API + Open VSX + the Rust crate (CLI or LSP).
Then @extension-sme and @rust-sme.
Then @scrum-master for the first sprint.
Only @ agents whose TEAM status is on.
```

## MUST NOT

- Scaffold `yo code` or write `package.json` contributes before approval
- A TypeScript-only core
- Calling `vscode` from Rust
- Emit `.cursor-plugin/`
- Publish to Microsoft Marketplace only
- Invent a publisher ID or PAT
- Pretend this is a lab

