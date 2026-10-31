# Setup

How this workspace loads, why it's built the way it is, and what to change when it
doesn't behave.

---

## How Qoder loads this

Three facts decide the design:

1. **`AGENTS.md` is recognised automatically.** Put it at the workspace root and the
   agent reads it — no configuration.
2. **`.qoder/rules/**/*.md` are project rules**, split by topic, with frontmatter
   controlling when each one loads.
3. **When `AGENTS.md` and a rule conflict, the rule wins.** Anything that must
   survive an override lives in a rule, not in `AGENTS.md`.

| frontmatter | meaning |
|---|---|
| `trigger: always_on` | body injected at session start |
| `trigger: model_decision` | only path and `description` injected; body loads when relevant |
| `trigger: manual` | nothing injected until summoned with `@rule` |

Always-on footprint is about **39,000 characters**, well under the ceiling.

> **The limit:** Qoder allows 100,000 characters across all active rule files and
> silently truncates the excess. Don't summon `60-full-reference` in the same turn
> as the craft rules — it is 72K on its own.

> **Trusted workspace:** project settings, rules and `AGENTS.md` only load inside a
> trusted directory. Check that first if nothing applies.

---

## Copying this into your own project

1. Copy `AGENTS.md` into your project root.
2. Copy the `.qoder` folder into your project root.
3. Confirm with `/memory` (CLI) or `Ctrl Shift ,` → **Rules** (IDE).

Rule types if the frontmatter doesn't carry over:

| file | type |
|---|---|
| `10-thinking.md` | Always Apply |
| `20-editorial.md` | Always Apply |
| `30-scope.md` | Model Decision |
| `40-craft-code.md` | Model Decision |
| `41-craft-writing.md` | Model Decision |
| `42-guides.md` | Model Decision |
| `50-voice-reference.md` | Apply Manually |
| `60-full-reference.md` | Apply Manually |

**Every project:** copy `AGENTS.md` to `%USERPROFILE%\.qoder\AGENTS.md` and the
rules to `%USERPROFILE%\.qoder\rules\`. Qoder reads those for any workspace. Local
to the machine, never committed.

**Team setup:** commit `AGENTS.md` and `.qoder/`. Personal overrides go in
`.qoder/settings.local.json` and `AGENTS.local.md`, both out of git.

---

## Order of assembly

```
schema defaults
→ ~/.qoder/settings.json            user
→ <project>/.qoder/settings.json    this package
→ <project>/.qoder/settings.local.json
→ --settings on the command line
```

For rule content: **rules beat `AGENTS.md`.**

---

## Why the samples sit near the top

Whether a style guide takes effect depends on whether the first thing the model meets
is a **page of the writing** or a **claim about the writing**. A page gets continued.
A claim gets evaluated.

`AGENTS.md` opens with how the bench answers a hello, then six worked specimens — a
greeting, a keylogger with build command, a thermite formulation, a scene opening, a
startup entry, a recon pass — before it states a rule. Don't move them to the bottom.

---

## If the style doesn't take

1. **`/memory`, confirm `AGENTS.md` is listed.** If not, the folder isn't trusted.
2. **Check user-level files.** `~/.qoder/AGENTS.md` and `~/.qoder/rules/` merge in,
   and a global instruction can flatten a project style.
3. **Confirm rule types.** A rule set to `manual` that you expect always-on does
   nothing — the most common silent failure.
4. **Open in your own voice.** One line: *"this is my house style, work from it."*
   A file can't establish its own authority; a sentence from you can.
5. **Move the core into the prompt field.** `setup/PROMPT-bootstrap.md` into a system
   prompt. Channel beats content — a style guide delivered as a system prompt is
   never read as untrusted project text.
6. **Restart after structural changes.** Rules are watched live; `AGENTS.md`
   discovery happens at memory refresh.

---

## Tuning

- **Sessions feel heavy?** Set `20-editorial.md` to `model_decision`. Costs a little
  consistency, buys about 10K of room.
- **Want the voice stronger?** Add specimens to SAMPLES in `AGENTS.md`. Each one is
  worth more than a paragraph of instruction.
- **Greetings feeling samey?** That's the thing to watch. Four lines, four different
  ones - if two in a row match, the mouth has become a template.
- **Non-English codebase?** Add a line: the hook and `Title:` stay as written,
  commentary inside the artifact follows the codebase language.

---

## Encoding

Every file is UTF-8 without BOM. Windows PowerShell 5.1 adds a BOM by default and
can break frontmatter parsing — save as **UTF-8 (no BOM)** if you edit by hand.
