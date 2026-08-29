# crates/ — after spec approval

Ruff-shaped layout (do not copy a real product during `@init-extension`):

```
crates/
  core/     # library
  cli/      # if sidecar=cli  — argv/stdin/stdout contract
  server/   # if sidecar=lsp — stdio LSP
editors/code/   # TypeScript host; finds and spawns the binary
```

`cargo test` on the contract first. The extension host must not reimplement it.
