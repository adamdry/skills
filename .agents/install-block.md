# The canonical install block

One install story, one wording. `README.md`, `.changeset/*`, and every page under `docs/` must say **this** and nothing else. Change it here first, then propagate.

## Only routes that have been run

A route goes in this file once someone has run it against this repo. Until then it stays out, and the agent in question installs through skills.sh.

The plugin installs from this repo's own marketplace, `.claude-plugin/marketplace.json` and `plugin.json`. Installed users see a release when the `version` in `plugin.json` changes. The Release workflow runs `npm run version`, which copies that field from `package.json`, so users get a release when the "chore: version skills" PR merges.

| Agent         | Route                  | Updates itself?                                              |
| ------------- | ---------------------- | ------------------------------------------------------------ |
| Claude Code   | `@adamdry` marketplace | Not checked. Update by hand with the commands below          |
| Everyone else | skills.sh              | No. Run `npx skills update`, and re-run `add` for new skills |

Verified 2026-11-19, on Claude Code 2.1.328: `claude plugin validate .` passes on both manifests with one expected warning (the root `CLAUDE.md` is not loaded as project context), and resolves `adamdry-skills:prompt-engineering`. Neither install route has been run yet, because the repo had no published commit at the time. Run both after the first merge to `main` and record the result here.

Nobody has run Codex, Copilot, VS Code or Gemini CLI against this repo. [mattpocock/skills](https://github.com/mattpocock/skills/blob/main/.agents/install-block.md) documents how each of them reads the same manifests, so that is where to start.

## Claude Code

<canonical-block name="claude-code">

```bash
claude plugin marketplace add adamdry/skills
claude plugin install adamdry-skills@adamdry
```

To update, run `claude plugin marketplace update adamdry`, then `claude plugin update adamdry-skills@adamdry`.

</canonical-block>

## Every other agent, or editable files (skills.sh)

<canonical-block name="other-agents">

```bash
npx skills@latest add adamdry/skills
```

To update, run `npx skills@latest update`, and re-run `add` to pick up new skills.

</canonical-block>

Use the single-skill form wherever one skill is named on its own.

<canonical-block name="skills-sh-one-skill">

```bash
npx skills@latest add adamdry/skills --skill=<name>
```

```bash
npx skills@latest update <name>
```

</canonical-block>

`skills@latest` is the pinned spelling everywhere.

## The two routes are exclusive

The plugin is a read-only bundle you subscribe to. skills.sh writes files you own and edit. Installing both leaves the user with every skill twice: always say "pick one".
