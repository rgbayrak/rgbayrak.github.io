---
layout: post
title: AI-assisted coding — without violating your DUAs or leaking sensitive data
date: 2026-03-27 00:00:00
description: Claude Code for Projects
tags: formatting code ai-assisted
categories: AI-posts
featured: true
---
 
> **A note before you read:** I am still learning this myself — this post is as much a note to myself as it is meant to be helpful to others. Everyone has their preferred tools, workflows, and institutional context — and the ai-assited coding landscape is changing fast. This post is not a prescriptive ruleset. It uses a specific project as a concrete example to illustrate the underlying principles of how a specific software (Claude Code) handles data, permissions, and memory. The right configuration for your situation depends on your tools, your datasets, and your institution's policies. Always check with your university's data governance office, IRB, and IT security team before using any AI coding assistant on governed research data.
 
---

**Context:** We are actively developing a browser-based visualization tool and testing it across three datasets each with different data governance requirements.

---

## Table of Contents

1. [The DUA Problem: What Claude Code Actually Sends to Anthropic](#1-the-dua-problem-what-claude-code-actually-sends-to-anthropic)
2. [Setting Up Claude Code in VSCode](#2-setting-up-claude-code-in-vscode)
3. [Understanding the Four Permission Modes](#3-understanding-the-four-permission-modes)
4. [Blocking Sensitive Data with Deny Rules and .claudeignore](#4-blocking-sensitive-data-with-deny-rules-and-claudeignore)
5. [How Claude's Memory System Works](#5-how-claudes-memory-system-works)
6. [Writing a CLAUDE.md for Your Visualization Tool](#6-writing-a-claudemd-for-your-visualization-tool)
7. [Practical Workflows for Daily Use](#7-practical-workflows-for-daily-use)
8. [Pre-Session Security Checklist](#8-pre-session-security-checklist)

---

## 1. The DUA Problem: What Claude Code Actually Sends to Anthropic

This is the section most guides skip. It is the most important one for you.

When you use Claude Code, the content of files it reads — either because you referenced them or because it explored your project directory — is sent to Anthropic's API as part of the request context. This is how the model understands your code. The practical implication: **any file Claude touches travels over the network to Anthropic's servers.**

### Our 3 test datasets — and their different risk profiles

We are testing our visualization tool against three datasets, each with a distinct governance situation:

**OpenNeuro ds000210** — Published on OpenNeuro under a CC0 license. Fully open, no DUA required. Claude Code can freely read BIDS metadata, `dataset_description.json`, sidecar JSONs, and non-imaging TSV files for this dataset. However, even here, participant-level NIfTI files should stay out of Claude's context — not for legal reasons, but because including them risks accidental transmission of brain data.

**OpenNeuro ds006644** — Also published on OpenNeuro under a CC0 license. Fully open, no DUA required. Same practical rules as ds000210 apply: BIDS metadata and sidecar JSONs are fine for Claude to see; participant-level files and NIfTI volumes should stay out of context.

**CNeuroMod Friends** — This is the critical one. The Courtois NeuroMod project's Friends dataset requires a formal Data Use Agreement. Researchers must apply for access and agree to specific terms covering data handling, storage, and sharing. The DUA prohibits sharing the data with third parties and requires it to remain on approved systems. **Routing any Friends dataset file contents through Claude Code — even de-identified metadata — is a potential DUA violation.** When working on features that touch CNeuroMod data paths, Claude must never see the actual data files, participant lists, or subject-level metadata.

A clean rule: **Claude Code should work on your tool's code, not your data.**

### Why even open datasets deserve caution

Even for CC0 datasets like ds000210, there are practical reasons to keep imaging data out of Claude's context. Brain data is sensitive by nature — it can be re-identified with sufficient auxiliary information, and norms around neuroimaging data sharing are still evolving. Keeping a hard habit of "Claude sees code, not data" also means you never accidentally copy a pattern from a relaxed context (open data) into a strict one (CNeuroMod).

> **When in doubt:** Check with your IRB or data governance office before using Claude Code on any DUA-governed data. AI coding assistants like Claude Code are third-party services under any standard DUA definition. **Letting Claude Code see file contents is sharing with a third party**.

---

## 2. Setting Up Claude Code in VSCode

### Installation

```bash
# Install the CLI (requires Node 18+)
npm install -g @anthropic-ai/claude-code

# Verify version — you want 2.1+
claude --version

# Install the VSCode extension
code --install-extension anthropic.claude-code

# Authenticate via OAuth or API key
claude
```

### How the VSCode extension helps

The Claude Code VSCode extension adds a sidebar panel with:

- **Diff viewing** — see exactly what Claude proposes to change before accepting
- **Inline accept/reject** — approve individual hunks, not just whole files
- **File navigation awareness** — Claude knows which file you have open

> **Critical:** Always `cd` into your *tool's source root*, not a data directory, before launching Claude Code. Claude's write access is restricted to the folder it was started in and its subdirectories. Starting in the right place is your first line of defense.

---

## 3. Permission Modes

Claude Code has four permission modes. For visualization tool development, you will primarily use two of them.

### Normal mode (default)

Claude asks for confirmation before every file write or shell command. Read operations are silent. This is the right default for everyday development — you see every action before it executes.

```bash
claude
```

Use this for all feature work that touches the tool's source code.

### Plan mode

Claude is restricted to read-only access and planning. No file modifications, no command execution. Use this when you want to think through an approach before committing to it — especially useful when designing how your viewer will handle multi-dataset configurations without accidentally touching any data file.

```bash
claude --permission-mode plan
```

**Best use case for this project:** You want Claude's analysis of what the code needs to do, without any execution risk while data paths are in the conversation.

---

## 4. Blocking Sensitive Data with Deny Rules and .claudeignore

This is the most important configuration you will do. Two complementary mechanisms: **`.claudeignore`** (scopes what Claude explores) and **`.claude/settings.json` deny rules** (controls what shell commands Claude can run).

### .claudeignore — keep imaging data out of context

Create `.claudeignore` in your project root. It works exactly like `.gitignore`. Files matching these patterns will not be read by Claude Code, even during directory traversal.

For our specific project — tool source code alongside or near test datasets — this file needs to be thorough:

```
# ── Data — never send to Anthropic API ── for data safety
data/
rawdata/
derivatives/
sourcedata/
*.nii
*.nii.gz
*.csv
*.tsv
*.json
*.mgz
*.mgh
*.dcm

# ── Credentials and secrets ── for data safety
.env
.env.*
.env.local
secrets/
credentials/
*.pem
*.key
*_credentials.json
*_token.json
.netrc

# ── Build artifacts and large binaries ── for performance reasons, not data safety
node_modules/
dist/
build/
*.wasm
*.so
*.dylib

# ── Viewer cache and temp files ── for performance reasons, not data safety
.cache/
tmp/
*.tmp
```

> **Important:** `.claudeignore` prevents Claude from *proactively* reading these files. If you explicitly write `@data/sub-01/anat/sub-01_T1w.nii.gz` in a prompt, Claude will still attempt to read it. The ignore file is a scoping tool, not an access control. Pair it with deny rules.

### Project specific deny rules — add to .claude/settings.json

Create `.claude/settings.json` at your project root (shared with your team via git).

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)"
    ],
    "deny": [
      "Bash(cat *friends*/*)",
      "Bash(ls */friends*/*)",
      "Bash(find */friends* *)"
    ]
  }
}
```

### Global deny rules — add to ~/.claude/settings.json

These apply across all your projects and protect your machine-level credentials:

```json
{
  "permissions": {
    "deny": [
      "Bash(curl *)",
      "Bash(wget *)",
      "Bash(rm -rf *)",
      "Bash(git push --force *)"
    ]
  },
  "autoMemoryEnabled": false
}
```

The `curl` and `wget` deny rules are critical. In Normal mode these require confirmation; in auto-accept mode they execute silently. Blocking them prevents any prompt-injected instruction from exfiltrating files over the network.

---

## 5. How Claude's Memory System Works

Claude Code has two persistence mechanisms across sessions. Both matter for a project that handles sensitive data.

### CLAUDE.md files — your standing instructions

`CLAUDE.md` is a plain markdown file at your project root (and optionally in subdirectories). Claude reads it at the start of every session as persistent context — your conventions, stack description, and hard data boundaries.

For a multi-dataset project, CLAUDE.md is where you name each dataset explicitly and state its governance status. Claude Code treats CLAUDE.md as its briefing document. If the rules are clear there, you don't need to re-state them every session.

CLAUDE.md files are **hierarchical**: a root-level file applies project-wide, subdirectory files apply locally. For a visualization tool, you might structure it as:

```
my-project/
├── CLAUDE.md                  ← tool-wide rules and dataset governance
├── src/
│   └── CLAUDE.md              ← frontend/backend conventions
└── tests/
    └── CLAUDE.md              ← test conventions, fixture data only
```

### Auto memory — notes Claude writes itself

Claude Code (v2.1.59+) accumulates notes from your sessions automatically: build commands it discovered, debugging patterns, your code style. These are stored locally at `~/.claude/projects/<project>/memory/` as plain markdown.

Auto memory is machine-local. But it can inadvertently record dataset paths, subject counts, or pipeline details that you would rather not have persisted. **Disable it for any project that touches DUA-governed data:**

```json
{
  "autoMemoryEnabled": false
}
```

Add this to `.claude/settings.json` in your project root. If you want auto memory for your tool's code conventions but need it off for data-adjacent sessions, you can toggle it manually with `/memory` in an active session.

To audit what Claude has already recorded:

```bash
# Find your project's memory directory
ls ~/.claude/projects/

# Read the memory index
cat ~/.claude/projects/<project-hash>/memory/MEMORY.md
```

You can edit or delete these files at any time.

---

## 6. Writing a CLAUDE.md for Your Visualization Tool

Here is a concrete CLAUDE.md tailored to our situation — a browser-based neuroimaging viewer being developed and tested against ds000210, ds000664, and CNeuroMod Friends.

```markdown
# my-project — Claude Code Instructions

## Test datasets and their governance

### OpenNeuro ds000210
License: CC0 (fully open). No DUA.
Path: data/ds000210/
Claude MAY read: dataset_description.json, task sidecar JSONs (e.g. *physio.json),
README. Claude must NOT read participant-level files or NIfTI volumes (Claude
cannot process them and they waste context).

### OpenNeuro ds006644
License: CC0 (fully open). No DUA.
Path: data/ds000664/
Same rules as ds000210.

### CNeuroMod Friends dataset
Governed by a formal Data Use Agreement — third-party sharing is prohibited.
Path: data/friends*
YOU MUST NOT read, reference, print, or include any content from:
- Any file under data/friends*
- participants.tsv, *_sessions.tsv, or any subject-level metadata
- Any sub-* directory
If a task requires understanding the CNeuroMod data structure, ask me to
describe it — do not attempt to read the files yourself.

## Hard rules (apply to all datasets)
- Never read .env, secret or any credentials file
- Never run curl or wget
- Never hardcode dataset paths — all paths via config or environment variables
- Never print or log subject IDs in generated code
- Never run git push

## Code conventions (??)
- All data I/O goes through src/io/ — do not add direct nibabel calls elsewhere
- Dataset adapter pattern: each dataset has an adapter in src/adapters/
- API routes in src/api/routes/ — one file per resource
- Frontend components in src/components/ — prefer functional components
- Types in src/types/ — shared between frontend and backend via openapi-ts

## Useful commands (??)
- Dev server: npm run dev (starts both FastAPI and Vite)
- Type generation: npm run gen:types
- Lint: ruff check . && eslint src/
- Format: black . && prettier --write src/
```

---

## 7. Practical Workflows for Daily Use

### Starting a session

```bash
# Always cd to the tool root — not a data directory
cd /path/to/my-project

# Confirm your ignore file is present
cat .claudeignore

# For new feature work: start in Plan mode first
claude --permission-mode plan
```

### The Plan → Review → Execute loop

For visualization tool work specifically, this loop prevents a category of subtle bugs where a frontend change breaks data loading for one dataset but not others.

1. **Plan mode** — [```claude --permission-mode plan```] describe what you want and ask Claude to lay out which files it will touch and what it intends to change. Review the plan.
2. **Normal mode** — [```claude```] execute with per-action approval. Watch the diffs in the VSCode sidebar before accepting.
3. **Test across all three datasets** — run your test suite with fixtures from each dataset configuration before committing.

Example Plan mode prompt for a visualization feature:

```
We need to add brush functionality to zoom into the waveform in the context window. 
Plan what changes are needed across the frontend. List the files you will touch. 
Do not make any changes yet.
```

### Dataset-aware prompting

When asking Claude to write code that will run against multiple datasets, be explicit about the differences between them — describe the structure from your knowledge, do not paste actual data.

Claude does not need to see the actual data files to write correct code — your description is enough, and it keeps the data out of context.

### Using @-references to control context

Give Claude exactly what it needs, nothing more. With a visualization tool it is easy to accidentally give Claude a broad reference that causes it to explore data directories.

```bash
# Good — specific source file references
src/components/QATab/signal-workspace/SignalWorkspace.vue
Fix the mismatch between brushed and visualized zoom-in behavior.

# Good — controlled multi-file context
@src/components/graph-display/SignalsDashboard.vue src/components/QATab/signal-workspace/SignalWorkspace.vue
Add the same cool feature for friends dataset following the same pattern.
Describe what it needs — I will fill in the data-specific details.

# Avoid !! — broad exploration that may hit data directories
"Look at the project structure and understand how data flows"
```

### Keeping API keys and credentials safe

Your visualization tool likely has access tokens for private dataset access (CNeuroMod requires authentication) and possibly cloud storage keys. Never put real tokens in any file Claude can see.

```bash
# .env — blocked by .claudeignore
CNEUROMOD_ACCESS_TOKEN=real_token_here
AWS_ACCESS_KEY_ID=real_key_here
OPENNEURO_API_KEY=real_key_here

# .env.example — checked into git, safe for Claude
CNEUROMOD_ACCESS_TOKEN=your_token_here
AWS_ACCESS_KEY_ID=your_key_here
OPENNEURO_API_KEY=your_key_here
```

Point Claude at `.env.example` when it needs to understand your config structure.

### Writing tests that do not use real data

For a visualization tool tested against real datasets, your test suite should use synthetic fixtures — not real participant files. Ask Claude to help generate them:

```
Write a pytest fixture that generates a minimal synthetic 1D physio waveform
with a valid BIDS sidecar JSON, suitable for testing our
volume loading pipeline without using any real participant data.
Place it in tests/fixtures/synthetic/.
```

This pattern lets Claude help you build a thorough test suite without ever needing to see real imaging data.

---

## 8. Pre-Session Security Checklist

Run through this before starting any Claude Code session on this project.

**Environment**
- [ ] `cd` into the tool source root — not a data directory
- [ ] `.claudeignore` present and includes `data/`
- [ ] `.claude/settings.json` deny rules include `curl`, `wget`
- [ ] `.env` blocked in `.claudeignore`

**CLAUDE.md**
- [ ] All three datasets named with their governance status
- [ ] CNeuroMod Friends section marked as DUA-restricted with explicit DO NOT READ

**Permission mode**
- [ ] Using Normal mode or Plan mode for all dataset-adjacent work
- [ ] Do not use Auto-accept or Bypass mode

**Auto memory**
- [ ] `autoMemoryEnabled: false` in `.claude/settings.json` OR
- [ ] Memory files reviewed: `~/.claude/projects/<project-name>/memory/MEMORY.md`

**Before committing**
- [ ] `git status` shows no data files, no `.env` staged
- [ ] No data appear in any changed source file
- [ ] Session closed (`/exit`) before pushing

---

## Closing thoughts

The one-time setup — `.claudeignore`, `settings.json` deny rules, a solid `CLAUDE.md` — takes about 20 minutes. After that, Claude Code becomes a genuinely productive collaborator for the parts of this work it is suited for: debugging, documentation, organization. 

Your DUA doesn't know what a CLAUDE.md is. But it does know what a third-party API is. Configure accordingly.

---

*Last updated: March 2026. Check the [official docs](https://docs.claude.com/en/docs/claude-code).