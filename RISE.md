# RISE version of Matt Pocock's skills

This branch is the version of [mattpocock/skills](https://github.com/mattpocock/skills) that RISE uses in Claude Code and Codex. People install it by cloning a tag, as described in [AI Assisted Coding](https://xwiki.rise.fx/xwiki/bin/view/AllCategories/Development/AI%20Assisted%20Coding). Claude Code loads the clone from `~/.claude/skills/mattpocock-skills`, Codex through a symlink in `~/.agents/skills`.

## Why a clone instead of a plugin

RISE keeps Codex plugins off. Codex can restrict plugin marketplaces, but it cannot restrict single plugins from OpenAI's remote catalog, which mostly offers app connectors such as Gmail, Google Drive and Slack. A clone works in both tools without plugins and pins one version for everyone. it/puppet!507 tried the plugin route for Codex and was closed for this reason.

## Why 1.2.3

Version 1.3 renames the domain documents `CONTEXT.md` and `CONTEXT-MAP.md` to `GLOSSARY.md` and `GLOSSARY-MAP.md` (upstream commit `d80fa0f`). RISE repositories use `CONTEXT.md`, and we are not migrating them yet.

The base is the 1.2.3 release commit `30204e6` (tag `v1.2.3`). Upstream `plugin.json` still said 1.2.3 for 76 commits after that, including the rename, so "1.2.3" from a plugin marketplace is not necessarily this commit.

## Changes against 1.2.3

1. **Removed `skills/in-progress/` and `skills/misc/`.** Claude Code loads only the skills listed in `.claude-plugin/plugin.json`, but Codex loads every `SKILL.md` in the clone. Without the removal, Codex users would get draft and personal skills that the plugin deliberately leaves out. Upstream describes the same limitation in `.agents/adr/0002-ship-as-a-claude-code-plugin.md`.
2. **Added `implement-spec`, `pr` and `retro` from 1.3.1** (upstream commit `2629ffc`). The RISE guide uses `/implement-spec` and `/retro`. `pr` is changed to read the domain language from `CONTEXT.md` instead of `GLOSSARY.md`; the other two are unchanged. All three are listed in `plugin.json`.

## Tags

| Tag | Content |
|---|---|
| `v1.2.3` | Upstream 1.2.3 release, unchanged. |
| `v1.2.3-rise` | Change 1 only. Superseded. |
| `v1.2.3-rise.1` | Changes 1 and 2. Superseded. |
| `v1.2.3-rise.2` | Changes 1 and 2 plus this file. Current. |

## Known limitation in Codex

Codex does not register skills marked `disable-model-invocation: true`, which are the 16 skills users start themselves, such as `ask-matt`. Typing `$ask-matt` still works because the model then searches the skill folders and reads the file, but it takes several tool calls. Claude Code registers these skills normally.

## Making a new RISE version

1. Commit the change on this branch. Keep upstream files as they are where possible, so a later upstream version merges cleanly.
2. Keep `plugin.json` and the `SKILL.md` files in step: every skill in the clone must be listed, and every listed skill must exist.
3. Tag the commit `v1.2.3-rise.<n+1>` and push branch and tag.
4. Update the tag in the install command on the wiki page.
