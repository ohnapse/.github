# ohnapse

**A terminal-native AI coding agent that stays autonomous but strictly bounded.**

`ohnapse` reads, searches, and edits files in your project from the command line,
running its agent loop inside a kernel sandbox behind a permission model you control.
It is built for developers who want an agent that can act on a real codebase without
handing it the keys to the machine.

```sh
brew install ohnapse/tap/ohnapse
```

```sh
curl -fsSL https://raw.githubusercontent.com/ohnapse/public/main/install.sh | sh
```

Both commands put `ohnapse` and `oh` on `PATH`. After the Homebrew install,
`brew upgrade ohnapse` works by short name. On Windows:

```powershell
irm https://raw.githubusercontent.com/ohnapse/public/main/install.ps1 | iex
```

Releases and the installer live in
[`ohnapse/public`](https://github.com/ohnapse/public); the formula lives in
[`ohnapse/homebrew-tap`](https://github.com/ohnapse/homebrew-tap).

```sh
export ANTHROPIC_API_KEY=sk-...
cd your-project
oh init
oh "add a doc comment to the Parse function in parser.go"
```

Run `oh` with no arguments for the interactive chat UI: a live transcript of the
conversation and its tool activity, a prompt composer, and a running token-cost meter.

## Why it's different

**Bounded by the kernel, not by good intentions.** File access is enforced by Landlock
on Linux and Seatbelt on macOS, so a tool call cannot reach outside your workspace even
if the model tries. Above that sits a three-tier permission model — `read-only`,
`auto-safe`, `full-auto` — with per-tool allow/deny/ask rules on top.

**Bring your own account, any account.** Anthropic, OpenAI, Azure OpenAI, and Google
Gemini are supported today. Configuration names an *account*, not a wire format, so
switching models on the same account is a one-line edit and nothing else changes.
Any OpenAI- or Anthropic-compatible endpoint works too.

**It measures instead of assuming.** Not every "compatible" endpoint really is — some
silently swallow tool calls and answer with an apology instead. `oh doctor` probes your
configured endpoint for streaming, tool calls, parallel tool calls, extended thinking,
and image input, then reports exactly what it found and caches the result.

**Parallel work, non-overlapping scopes.** Larger jobs are delegated to focused
sub-agents that run concurrently under a shared budget, each confined to a declared
slice of the tree, so two of them can never fight over the same file.

**One small binary.** Written in Go, cgo-free. It runs on Linux, macOS, and
Windows and starts instantly.

## Configuration

Settings live in a git-ignored `.ohnapse/settings.json` in your project, created by
`oh init`. Reference the published JSON Schema for validation and autocompletion in any
editor that supports it:

```json
{
  "$schema": "https://raw.githubusercontent.com/ohnapse/public/main/schemas/ohnapse-settings.schema.json"
}
```

## Availability

**Alpha**, and moving quickly. Everything described above works today. A hosted
subscription that replaces per-vendor API keys with a single account is on the way.

If you are running it on a real repo, [file a bug or a session report](https://github.com/ohnapse/public/issues/new/choose).
Do not paste API keys.

---

Built by [Kolosys](https://github.com/kolosys).
