# Claude Code Desktop App — Cheatsheet (for experienced engineers)

**Same engine, different surface.** The desktop app is the **Code** tab inside the Claude Desktop app. It runs the identical agent loop as the terminal CLI and shares everything: CLAUDE.md, `.claude/` settings, MCP servers, hooks, skills, plugins, permission rules, and model access. So the whole "six primitives" model from the CLI cheatsheet applies unchanged. This doc only covers what the desktop surface adds on top.

The desktop app has **three tabs**: **Chat** (like claude.ai), **Cowork** (agentic knowledge work / Dispatch), and **Code** (software development — this doc).

---

## 1. Install & first launch

- Download from **claude.ai/download** (macOS, Windows x64/ARM64, Linux via apt/.deb).
- macOS 11+; Windows 10+ (install **Git for Windows** first, then restart the app).
- Requires a paid plan (Pro/Max/Team/Enterprise) for Code.
- Open the app → **Code** tab → pick a project folder → start.

Note: Code runs on Intel Macs too; only **Cowork** needs Apple Silicon.

---

## 2. Configure 4 things before your first message

The prompt area lets you set, per session:
1. **Environment** — where Claude runs: **Local** (your machine), **Remote** (Anthropic cloud sandbox), or **SSH** (a remote machine you manage; supported from Mac and Linux).
2. **Project folder** — the repo/folder Claude works in.
3. **Model** — from the dropdown (tied to your account, not the surface).
4. **Permission mode** — Plan / Accept edits / Ask permissions (see below).

---

## 3. The panes (this is the real difference vs. CLI)

Everything is **drag-and-drop** — arrange them in whatever grid fits your work:

| Pane | What it does |
|---|---|
| **Chat** | The conversation / agent driver. |
| **File structure sidebar** | Live project tree; updates as Claude creates/edits/deletes files. Your spatial map. |
| **Diff viewer** | Fast visual review of every change, per turn and uncommitted. Rebuilt for large changesets. |
| **In-app file editor** | Open files, make spot edits yourself, save — no bouncing to another editor. |
| **Integrated terminal** | Run tests/builds alongside the session. |
| **Preview** | Embedded browser for your running app (also opens HTML/PDF in-app). |

A **session sidebar** lists every active and recent session so you can move between repos as results arrive.

---

## 4. Permission modes (as buttons, not Shift+Tab)

Same modes as CLI, surfaced in the UI:
- **Plan** — read-only: explore + propose, no changes.
- **Accept edits** — auto-approves file edits, still asks for shell commands.
- **Ask permissions** — prompts before machine-touching actions (the default).
- **Auto** — hands-off, classifier-gated.

Recommended flow: start in **Plan**, approve the plan, then switch to **Accept edits** to execute.

Cloud (Remote) sessions expose **Accept edits / Plan / Auto** — "Accept edits" is the default there because the sandbox pre-approves edits. **Bypass permissions isn't offered in the cloud** since it's already sandboxed. Enterprise admins can restrict which modes are available.

---

## 5. Parallel sessions with Git isolation

The headline feature of the redesign. You're the orchestrator: kick off a refactor in one repo, a bug fix in another, a test pass in a third, and check on each as results land. Each parallel session gets:
- its **own Git worktree** (isolated branch/working dir — no stepping on your main tree), and
- its **own context window**.

⚠️ **Cost reality:** N parallel sessions ≈ N× token burn. On Pro you can hit limits fast running several at once; factor this into plan choice.

---

## 6. Live preview + self-verification

Claude can start a dev server and open the **embedded browser** to verify its own work (frontend *and* backend):
- Takes screenshots, inspects the DOM, clicks elements, fills forms, reads server logs, and fixes what it finds.
- Start/stop servers from the **Preview** dropdown; **Persist sessions** keeps cookies/localStorage across restarts so you don't re-login.
- Config lives in **`.claude/launch.json`** (auto-generated; edit for custom dev commands, non-standard ports, or monorepo frontend+backend).

---

## 7. Review & ship without leaving the app

- **Review code button** (in the diff view): Claude scans your local diffs and leaves **inline comments** on real issues — logic errors, compile errors, security bugs — *before* you push. Act on them, get a fresh diff, repeat. (This is the solo-dev feature, distinct from the Team/Enterprise multi-agent PR review system.)
- **PR monitoring:** after you open a PR, a **CI status bar** appears in the session; it can watch checks and support auto-merge, so you're not refreshing a browser tab.
- **Connectors:** wire in GitHub, Slack, Linear, etc. (MCP under the hood).

---

## 8. Side chats & remote control

- **Side chat** (`/btw`): ask a tangential question that uses the session's context without derailing the main thread.
- **Dispatch:** message a task from your **phone** → it spawns a Code session on your desktop. Good for kicking off work while away from your desk.
- **Computer use** (macOS, beta): let Claude drive native apps via AppleScript/shell with per-action approval.

---

## 9. Desktop vs. CLI — when to use which

**Use Desktop when** you want: visual diff review, parallel sessions in one window, automatic worktrees, live app preview, in-app file editing, PR/CI monitoring, file attachments, side chats, Dispatch from phone, or you'd rather not live in the terminal.

**Drop to CLI when** you need: scripting, `claude -p`, shell piping, CI/cron/launchd, the Agent SDK, or third-party model routing.

They read the same `.claude/` config, so you can freely mix — set up CLAUDE.md, skills, and hooks once and both surfaces honor them.

---
*Verified against Anthropic's Claude Code desktop docs and the April 2026 redesign announcement, July 2026. The desktop app ships frequently — check the in-app what's-new and code.claude.com/docs/en/desktop for the latest.*
