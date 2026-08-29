# Extension spec

## Kind

TRACK.md value and what that means for `contributes`.

## Architecture

- `editors/code/src/extension.ts` — activate/deactivate, find binary, spawn CLI or LSP client
- `crates/core` + `crates/cli` or `crates/server`
- Track specialist work (webview / DAP / …) does not replace the Rust core

## Sidecar

cli | lsp — argv/stdio contract written here.

## Open VSX

## Open VSX

- Package identity
- Homepage (domain, not GitHub README, if verification is a goal)
- Dual-publish ID lock (if dual)

## Risks

Conflicts with Cursor AI, activation cost, network, enterprise allowlists.
