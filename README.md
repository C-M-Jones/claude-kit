# claude-kit (fork)

> **This is a fork of [johnnyvizz/claude-kit](https://github.com/johnnyvizz/claude-kit).** savvy-flow and savvy-progress are [johnnyvizz](https://github.com/johnnyvizz)'s work, and all credit for them goes to johnnyvizz. This fork only adds some extras to savvy-progress, listed [below](#whats-different-in-this-fork). They've been offered back to the original as pull requests ([#14](https://github.com/johnnyvizz/claude-kit/pull/14), [#15](https://github.com/johnnyvizz/claude-kit/pull/15)). If you just want the plugins as their author made them, use [the original repository](https://github.com/johnnyvizz/claude-kit).

johnnyvizz's public skills and mods for [Claude Code](https://claude.com/claude-code), in one place. Every item is a standalone plugin under `plugins/`: install only what you need.

| Plugin | Type | What it does |
| --- | --- | --- |
| [savvy-flow](plugins/savvy-flow) | skill + agents | `/savvy-flow <task>`: the session model plans, delegates to tiered worker subagents and reviews their work. |
| [savvy-progress](plugins/savvy-progress) | mod | A progress bar above the prompt and a live agents panel. Made for savvy-flow, shows any subagents. |
| [session-band](plugins/session-band) | mod | A band above the prompt: turn, speed, cost, prompt cache countdown and context; notifies before the cache expires. |

savvy-flow and savvy-progress work on their own and light up together: with both installed, the flow drives the bar and workers report their progress to the panel.

## What's different in this fork

Additions to **savvy-progress** only. savvy-flow is unchanged.

- **Solo steps:** work Claude does itself, without subagents, shows as steps with a status, a one-line summary and a timer.
- **Step avatars:** each step gets a costumed crab like the subagent rows. The active one walks, and the bar's crab dresses as the current step.
- **Cost per step:** a finished step shows how long it took and what it added to the session's cost and context.
- **Files and diffs per step:** open a step to see the files it read or changed, reveal them in Finder, and view a diff of exactly what that step changed.
- **Session and limit tiles:** the session's API cost, context use, and 5-hour and weekly limits with reset times.
- **Background commands:** live rows for background shell commands, with progress and a pop-up notice when one finishes, fails or goes quiet.
- **Crab moods:** a failed subagent's crab lies on its back under circling stars, and the bar's crab celebrates in confetti when a task finishes.
- **Code split into modules** (`hooks/*.ts`) with tests (`hooks/*.test.ts`).

## Install

### From the marketplace

In Claude Code:

```
/plugin marketplace add C-M-Jones/claude-kit
/plugin install savvy-flow@claude-kit
/plugin install savvy-progress@claude-kit
```

This adds the fork. For the original, use `/plugin marketplace add johnnyvizz/claude-kit` instead. Both are named `claude-kit`, so add only one of them.

Restart the session afterwards. Plugin skills and agents are namespaced: the skill is `/savvy-flow:savvy-flow`, the agents `savvy-flow:savvy-careful` and so on.

### By hand

```bash
git clone https://github.com/C-M-Jones/claude-kit.git ~/claude-kit
```

**A skill** (no plugin system involved, names stay short: `/savvy-flow`, `savvy-careful`): copy its folders into your Claude Code config.

```bash
cp -R ~/claude-kit/plugins/savvy-flow/skills/savvy-flow ~/.claude/skills/
```

```bash
cp ~/claude-kit/plugins/savvy-flow/agents/*.md ~/.claude/agents/
```

**A mod**: load the folder for one session,

```bash
claude --plugin-dir ~/claude-kit/plugins/savvy-progress
```

or for every session (including the desktop app) through `env` in `~/.claude/settings.json`. Several folders are separated with `:`; if the variable is already set, append to it rather than replacing it:

```json
{
  "env": {
    "CLAUDE_CODE_PLUGIN_DIRS": "~/claude-kit/plugins/savvy-progress"
  }
}
```

`git pull` in the clone updates a mod loaded this way; copied skills need copying again.

Mods are built on function hooks (`hooks/register.tsx`), an early-access Claude Code API: tested on Claude Code 2.1.286, and it may change between releases. If a mod installed from the marketplace does not show up, load it by hand as above.

## Layout

```
.claude-plugin/marketplace.json   the catalog `/plugin marketplace add` reads
plugins/<name>/
  .claude-plugin/plugin.json      manifest
  skills/<skill>/SKILL.md         a skill
  agents/*.md                     subagents it ships
  hooks/hooks.json                a mod: points at the hooks module
  hooks/register.tsx              a mod: the module itself
  types/index.d.ts                a mod: its $.state contract
```

Check a plugin before publishing: `claude plugin validate plugins/<name>`, and the catalog with `claude plugin validate .`.

## Credits and license

Created by [johnnyvizz](https://github.com/johnnyvizz) ([original repository](https://github.com/johnnyvizz/claude-kit)). The fork's additions are by [C-M-Jones](https://github.com/C-M-Jones).

MIT, under the original's [license](LICENSE), copyright johnnyvizz.
