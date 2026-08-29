# Cursor Extension Factory

A **factory**, not a lab.

This repo is boilerplate for a **VS Code-compatible extension** that runs in [Cursor](https://cursor.com).
Cursor's team implements from a spec you write in the first session.

Cursor loads third-party extensions from **[Open VSX](https://open-vsx.org)** (see [Cursor: Extensions](https://cursor.com/help/customization/extensions)).
This factory targets that path. It is **not** a Cursor **plugin** factory (rules/skills/`.cursor-plugin`). For plugins, see [cursor-tws-plugin](https://github.com/jxtngx/cursor-tws-plugin) as an example product, not this boilerplate.

> **Lab** = student writes the code. Mentors quiz and review.
> **Factory** = you define requirements. Chief Architect, SME, Scrum, and engineers ship tickets.

Sister factories: [cursor-agent-factory](https://github.com/jxtngx/cursor-agent-factory) · [cursor-fullstack-factory](https://github.com/jxtngx/cursor-fullstack-factory) · [cursor-swift-factory](https://github.com/jxtngx/cursor-swift-factory) · [cursor-deep-learning-factory](https://github.com/jxtngx/cursor-deep-learning-factory).

If you wanted to *learn* the VS Code API by typing every contribution yourself, that would be a lab. This is not that.

---

## First command

Open this repo in Cursor and run:

```
@init-extension
```

That command:

1. Asks **what kind of extension** (one product, one kind)
2. Asks **CLI sidecar vs stdio LSP** (both still TS host + Rust binary)
3. Asks **Cursor-first (Open VSX) vs dual-publish**
4. Walks a **requirements interview**
5. Writes the spec, `TRACK.md`, and **rewrites `.cursor/TEAM.md`**
6. Hands off to `@chief-architect` → `@extension-sme` → `@rust-sme` → `@scrum-master`

Do not ask an engineer to `yo code` before the spec exists.

---

## Extension vs plugin

| | This factory | Cursor plugin |
| --- | --- | --- |
| Manifest | `package.json` (`engines.vscode`, `contributes`) | `.cursor-plugin/plugin.json` |
| Runtime | Extension host (VS Code API) | Agent rules / skills / commands |
| Distribution | Open VSX (Cursor marketplace proxy) | Cursor plugin install |
| Docs | [Extensions](https://cursor.com/help/customization/extensions) · [VS Code API](https://code.visualstudio.com/api) | [Plugins](https://cursor.com/docs/plugins) |

Do not emit a `.cursor-plugin` tree from this factory.

---

## Opinionated stack (not optional)

**TypeScript + Rust, Ruff-shaped.** Not TS-only. Not Rust-only `activate()`.

| Layer | Choice |
| --- | --- |
| Editor host | TypeScript `vscode` API (`editors/code/`) |
| Product logic | Rust crate(s) (`crates/`) — CLI and/or stdio LSP, like [Ruff](https://docs.astral.sh/ruff/) |
| Ship | Native binary **bundled** per platform (or a documented GitHub-release download). Same idea as Ruff / rust-analyzer |
| Build TS | `esbuild` (or `tsc` if the spec forbids bundling) |
| Build Rust | `cargo`, stable, documented target triples |
| Test | `@vscode/test-electron` + `cargo test` |
| Package | `@vscode/vsce` |
| Publish (Cursor) | `ovsx` → [Open VSX](https://open-vsx.org) |

You may not:

- Implement the real work in TypeScript “for now”
- Skip the Rust crate because the first command is a UI stub
- Call the VS Code API from Rust (no supported extension-host bindings)
- Ship Microsoft Marketplace only

Sidecar talk: subprocess (Ruff-classic CLI) or stdio LSP (`ruff server` / rust-analyzer). WASM is a later milestone in the spec, not the default.

---

## Kind → team

`@init-extension` **updates the agent team**. Always-on vs track specialists:

| Kind (`TRACK.md`) | Extra agents (on) | Parked |
| --- | --- | --- |
| `commands-ui` | webview-engineer | lsp, dap, theme |
| `language` | lsp-engineer | webview (unless spec adds one), dap, theme |
| `debugger` | dap-engineer | lsp, theme |
| `formatter` | lsp-engineer (document formatting) | dap, theme |
| `theme` | theme-engineer | lsp, dap, webview |
| `notebook` | notebook-engineer | dap, theme |
| `ai-tool` | ai-extension-engineer | theme, dap |
| `tree-scm` | tree-engineer | theme, dap |

Core (always on): Product Manager, Chief Architect, Extension SME, **Rust SME**, Scrum Master, Extension Engineer (TS), **Rust Engineer**, Platform Engineer, Test Engineer, Reviewer.

Roster file: `.cursor/TEAM.md` (generated). Agents not listed **stay silent** unless the spec adds them later.

---

## After init (typical)

```
@init-extension
  → approve technical requirements
@chief-architect
@extension-sme
@scrum-master
@run-ticket-plan
@review-extension
```

Later: `@publish-openvsx` (never before the spec and tests).

---

## Cursor marketplace facts (do not skip)

From [Cursor: Extensions](https://cursor.com/help/customization/extensions):

- Cursor uses **Open VSX**, proxied through `marketplace.cursorapi.com`, with malware/supply-chain analysis.
- Publisher.extension IDs can differ from the Microsoft Marketplace — treat IDs like dependencies.
- Dual-publish: **same extension ID** on Open VSX and elsewhere.
- Publisher verification: public website (not a GitHub README) + Open VSX homepage + [forum thread](https://forum.cursor.com/c/showcase/extension-verification/23).
- Do not fight Cursor's AI features; disable-on-conflict is a product requirement if you inject completions.

Official authoring API remains [VS Code Extension API](https://code.visualstudio.com/api).

---

## Repo layout (this boilerplate)

```
.cursor/
  commands/     init-extension, launch-product-discovery, run-ticket-plan, review-extension, publish-openvsx
  agents/       core (incl. rust-engineer) + track specialists
  templates/    requirements, team roster
  plans/project-init/
crates/         walking-skeleton notes for the Rust binary (after spec)
editors/        notes for the TS host (after spec) — see templates/<kind>/
templates/
  commands-ui/ …
TRACK.md
.cursor/TEAM.md
```

---

## Related repos

| Repo | Kind |
| --- | --- |
| [cursor-tws-plugin](https://github.com/jxtngx/cursor-tws-plugin) | Product — Cursor **plugin**, not this factory |
| [cursor-swift-factory](https://github.com/jxtngx/cursor-swift-factory) | Factory — Swift apps |
| [cursor-agent-factory](https://github.com/jxtngx/cursor-agent-factory) | Factory — LangChain |
| [cursor-fullstack-factory](https://github.com/jxtngx/cursor-fullstack-factory) | Factory — fullstack |
| [cursor-rust-lab](https://github.com/jxtngx/cursor-rust-lab) | Lab |

---

## License

Apache-2.0. See [LICENSE](LICENSE).
Not affiliated with Anysphere or Microsoft.
