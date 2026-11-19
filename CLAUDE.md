Skills are organized into bucket folders under `skills/`:

- `engineering/`: code work
- `productivity/`: non-code workflow tools
- `misc/`: kept around but rarely used, not promoted
- `in-progress/`: beta: public on purpose, feedback wanted, not shipped in the plugin
- `deprecated/`: no longer used

Every skill in `engineering/` or `productivity/` (the **promoted** buckets) must have a reference in the top-level `README.md` and an entry in `.claude-plugin/plugin.json`'s `skills` array (the Claude Code plugin ships exactly the promoted set). Skills in `misc/`, `in-progress/`, and `deprecated/` must not appear in either.

Install commands are copied verbatim from [.agents/install-block.md](./.agents/install-block.md). `.claude-plugin/marketplace.json` is the repo's own marketplace, and the plugin installs from it. Run `claude plugin validate .` after touching either manifest. One warning is expected, that `CLAUDE.md` at the plugin root is not loaded as project context, which is why `--strict` fails here (and upstream).

Each skill entry in the top-level `README.md` must link the skill name to its `SKILL.md`.

Each bucket folder has a `README.md` that lists every skill in the bucket with a one-line description, with the skill name linked to its `SKILL.md`. The promoted buckets' `README.md`s and the top-level `README.md` group entries into **User-invoked** and **Model-invoked**; non-promoted bucket `README.md`s (`misc/`, `in-progress/`) use a flat list.

Skills in `engineering/` and `productivity/` also have a human-facing docs page at `docs/<bucket>/<skill-name>.md` (the docs tree mirrors those two bucket folders under `skills/`). When you add, rename, or change the behaviour of a skill in `engineering/` or `productivity/`, create or re-sync its docs page following [.agents/writing-docs.md](./.agents/writing-docs.md). A finished page carries four sections: **What it does**, **When to reach for it**, **Common questions**, and **It's working if**. `writing-docs.md` holds the template and the section order. Skills in the non-promoted buckets get **no** docs page.

Every `SKILL.md` is either user-invoked (`disable-model-invocation: true` plus `policy.allow_implicit_invocation: false` in `agents/openai.yaml`, reachable only by the human) or model-invoked (model- or user-reachable). See [.agents/invocation.md](./.agents/invocation.md).

Every change to a skill ships with a changeset: run `npx changeset`, pick the bump, and describe the change in one or two sentences that name the skill. The Release workflow turns merged changesets into a "chore: version skills" pull request, and `npm run version` keeps `.claude-plugin/plugin.json`'s `version` equal to `package.json`'s.

Changes reach `main` through a pull request. The Check workflow must pass first: run `npm run check-frontmatter` and `npm run check-plugin-version` locally before you push.

To (re)link every skill outside `deprecated/` and `misc/` into the local harness skill directories (`~/.claude/skills`, `~/.agents/skills`), run `scripts/link-skills.sh`. Each entry is a symlink into this repo, so a `git pull` keeps installed skills current; re-run the script after adding, removing, or renaming a skill. The script replaces any real directory of the same name with a symlink.

No em-dashes anywhere in this repo's prose (`SKILL.md` files, docs, `README.md`, `CHANGELOG.md`, ADRs, changesets, code comments). Where a sentence reaches for one, rewrite it instead with a comma, colon, period, parentheses, or a conjunction, whichever the sentence actually wants; never do a blind character substitution. The one exception is text quoted or extracted verbatim from a source, such as `skills/engineering/prompt-engineering/references/`: a quotation keeps its author's punctuation.

A skill that bundles someone else's text says so in its own `SKILL.md`, and `CREDITS.md` records the source and its licence. The repo's MIT licence covers only what was written here.

Nothing from a client or an employer goes into this repo: no names, no paths, no code, no test cases built from their work. The repo is public.

Every skill that dispatches a subagent says to start it with a fresh context window. Some harnesses fork the parent conversation by default, and a fork inherits the parent's context, which is what the subagent was meant to escape.

The layout and conventions here are adapted from [mattpocock/skills](https://github.com/mattpocock/skills). [ADR 0001](./.agents/adr/0001-adopt-mattpocock-skills-conventions.md) records what was taken and what differs.

## Agent skills

### Triage labels

Canonical names, unchanged. See `docs/agents/triage-labels.md`. Issues are judged against [`SCOPE.md`](./SCOPE.md).
