# Adam Dry Skills

A collection of agent skills (slash commands and behaviours) loaded by a coding agent. Skills are organized into buckets, and the promoted ones ship as a Claude Code plugin.

## Language

**Skill**:
A folder holding a `SKILL.md` and whatever it needs beside it: reference files, scripts, test cases.
_Avoid_: command, prompt

**Bucket**:
A folder under `skills/` that groups skills by how settled they are and what they are for: `engineering`, `productivity`, `misc`, `in-progress`, `deprecated`.

**Promoted**:
In `engineering/` or `productivity/`. A promoted **Skill** is listed in the top-level `README.md`, ships in the plugin, and has a **Docs page**.

**User-invoked**:
Reachable only when a person types the **Skill**'s name.

**Model-invoked**:
Reachable by the agent on its own when the task fits, and by a person typing its name.

**Docs page**:
The human-facing page for one promoted **Skill**, at `docs/<bucket>/<skill-name>.md`.

**Changeset**:
A small file under `.changeset/` that describes one change and the version bump it needs. Merged changesets become the next release.

## Relationships

- A **Bucket** holds many **Skills**
- A **Skill** is **User-invoked** or **Model-invoked**, never both
- A **Promoted** **Skill** has exactly one **Docs page**
