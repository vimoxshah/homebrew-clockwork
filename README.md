# Clockwork Homebrew tap

The calendar where your AI agents show up for work. Book agent jobs on a real
calendar; Clockwork runs them unattended in an isolated, sandboxed git worktree
and files a report you can read.

## Install

```bash
brew tap vimoxshah/clockwork
brew install --cask --no-quarantine clockwork
```

### Why `--no-quarantine`

Clockwork is **not notarised by Apple**. A Developer certificate is $99/year and
this is an early build, so the app is ad-hoc signed. macOS quarantines anything
downloaded from the internet and refuses to open unsigned apps — usually with a
misleading "the app is damaged" message.

`--no-quarantine` skips that. Homebrew still verifies the DMG's SHA-256 against
the hash pinned in the cask, so you are trusting a binary whose hash is checked
for you — which is meaningfully better than hand-running `xattr` on a file you
never verified.

If you already installed without the flag:

```bash
xattr -dr com.apple.quarantine /Applications/Clockwork.app
```

## Requirements

- macOS 14 (Sonoma) or later, Apple silicon
- Node 22+
- At least one provider CLI on your PATH: `claude`, `codex`, `opencode` or `hermes`

## What it can reach

Worth knowing before you bypass Gatekeeper for any tool that runs code unattended:

- Runs execute in a macOS Seatbelt sandbox inside a per-run git worktree; writes
  are restricted to that worktree
- Credential paths (`~/.ssh`, `~/.aws`, `~/.gnupg`, browser cookies, shell
  history) are denied to the run
- The run environment is an allowlist — no `SSH_AUTH_SOCK`, no provider tokens
- The daemon binds `127.0.0.1` only
- Your provider API keys stay in the macOS Keychain

These are enforced and covered by tests, not aspirations.

## Links

- Website: https://clockworkd.com
- Issues: https://github.com/vimoxshah/homebrew-clockwork/issues

This tap contains only the cask formula. Clockwork itself is closed source.
