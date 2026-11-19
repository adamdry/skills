# Adopt the mattpocock/skills repo conventions

This repo starts with one skill and is meant to grow. Rather than invent a layout, it adopts the one in [mattpocock/skills](https://github.com/mattpocock/skills) (MIT), as that repo stood on 2026-11-19. The conventions there have been worked out against a large catalogue and a large audience, and agents that already know that repo find their way around this one.

## What was taken

- **Buckets.** `skills/engineering/`, `productivity/`, `misc/`, `in-progress/` and `deprecated/`, with `engineering/` and `productivity/` as the promoted set.
- **Invocation split.** Every skill is user-invoked or model-invoked, set in the frontmatter and mirrored in `agents/openai.yaml`.
- **Plugin.** `.claude-plugin/plugin.json` lists the promoted skills one by one, and `.claude-plugin/marketplace.json` makes the repo its own single-plugin marketplace.
- **Docs pages.** One human-facing page per promoted skill at `docs/<bucket>/<skill-name>.md`, on a fixed template.
- **Releases.** Changesets, with a Release workflow that opens a "chore: version skills" pull request and a script that copies the version into `plugin.json`.
- **Checks.** A Check workflow that parses every `SKILL.md`'s frontmatter with the parser the `skills` installer uses.
- **Issue handling.** Issue forms, a `needs-triage` label on every new issue, a 14-day close on `needs-info`, and a `SCOPE.md` bar.
- **Prose rule.** No em-dashes.

The scripts and workflows are copied with small edits. `CREDITS.md` carries the upstream licence notice.

## What differs

- **Install routes.** That repo is listed in Claude Code's official marketplace. This one is not, so Claude Code installs from the repo's own marketplace. Only the Claude Code route and skills.sh are documented, because they are the only ones that have been run. See [install-block.md](../install-block.md).
- **Docs pages are read on GitHub.** There is no published site, so links are repo-relative and the page template drops the rules that exist for the upstream site.
- **No router skill and no setup skill.** With one skill there is nothing to route between and nothing to configure. Add a router when the user-invoked skills are too many to remember.
- **Evals live beside the skill.** `prompt-engineering` keeps test cases in its own `evals/` folder, in the layout `claude plugin eval` reads. Upstream has none. They are not run in CI, because each run is a billed model call.
- **Bundled source text.** `prompt-engineering` ships the text of the paper it draws on, under that paper's CC BY 4.0 licence. The em-dash rule exempts verbatim text for this reason.
- **Main is protected.** Upstream goes through pull requests by habit. Here a ruleset requires a pull request and a passing Check before anything reaches `main`. A repo admin may merge a pull request past the Check, because the "chore: version skills" pull request is opened by the Release workflow and a pull request opened by a workflow never starts the Check.
- **The Check workflow also runs `check-plugin-version`.** Upstream ships the script but does not run it in CI.

## Consequences

When upstream changes a convention, nothing here changes automatically. Re-read that repo before a large restructure, and update this record when a convention is dropped or a new one is taken.
