# Writing docs pages

Every skill in `engineering/` and `productivity/` has a human-facing **docs page** at `docs/<bucket>/<skill-name>.md`. The docs tree mirrors those two bucket folders under `skills/`. The page is read on GitHub. It is not the skill and not a copy of `SKILL.md`. Only these two buckets are promoted; the rest (`misc/`, `in-progress/`, `deprecated/`) ship no docs page.

A skill the agent never fires for you is one *you* have to remember exists, and remember when to reach for. That memory is **cognitive load**. The job of a docs page is to relieve it: to orient one reader around one skill so they can hold it in their head, know when to reach for it, and see where it sits among the others.

Act whenever a promoted skill is added, renamed, or has its behaviour changed: create or re-sync its docs page. A rename moves the file too (`docs/<bucket>/<old>.md` → `docs/<bucket>/<new>.md`). A skill that moves between `engineering/` and `productivity/` moves its docs file to the matching folder. A skill moving out of a non-promoted bucket gains a page; one moving the other way loses it. A promoted skill that is removed outright keeps its page as an **archived** page: leave the body as it was and open it with a blockquote notice (`> **Archived.** ...`) naming the version it was removed in and any replacement.

Links are repo-relative, so they work on GitHub and in a clone. A link to another skill's page points at `../<bucket>/<name>.md`; a link to a skill points at its `SKILL.md`.

There is no H1. The file name is the title.

## Page structure

Fill the template below, keeping its order. The **fixed frame** (`## What it does`, `## When to reach for it`, `## Where it fits`) appears on every page. `## Prerequisites` and the free-form substance sections carry only what this particular skill needs; delete the rest.

Four sections make a page worth reading: `What it does`, `When to reach for it`, `Common questions`, `It's working if`. The first two orient the reader; the last two are where the page stops summarising the skill and starts answering the reader's own situation. Treat a page that clears neither of the last two as unfinished, not as finished-and-short.

**A page carries no install commands.** Install wording lives in one place, [the install block](./install-block.md), and the top-level `README.md` copies it. A page that writes the commands out again is a second copy that drifts.

<page-template>

## What it does

One or two plain-language paragraphs. Lead with the skill's one-sentence job, then state the **defining constraint**: the single fact that makes this skill behave differently from the obvious default (for `prompt-engineering`: every claim it makes is quoted from the paper's text, never recalled). Write it as a plain declarative sentence, never a labelled aside like "The defining constraint:" or "The key thing:"; the formula reads as filler. This line is the most valuable on the page; never omit it.

## When to reach for it

How and when you reach for the skill, in two beats that are both effectively always present:

- **Invocation mode.** State whether you type it or the agent fires it. A user-invoked skill: "You invoke this by typing `/<name>`, and the agent won't reach for it on its own." A model-invoked skill: "Type `/<name>`, or the agent reaches for it automatically when a task fits."
- **Trigger boundary.** The index entry: "reach for this when …". Where the skill is confusable with a sibling, in this repo or a well-known one elsewhere, add the other half: "for <X> instead, use <sibling>."

## Prerequisites

Optional: include only when the skill needs something in place to be functional; omit the heading entirely otherwise. Covers a **workspace it writes into**, **prior setup**, or **repo-specific tooling**. A stateless skill that runs anywhere has no prerequisites, so drop the section.

## <free-form middle>

One to three short sections, in the skill's *own vocabulary*, that make it click. Choose whatever headings fit the skill: the loop it runs, the artifact it produces, the fork it makes, the one anti-pattern it kills. There is no prescribed heading; skills are too heterogeneous for one.

The single non-negotiable: **surface the skill's leading word / defining idea**. It pays off twice: the reader learns what the skill *is*, and learns the word they'll later think with to *reach for* it.

## Common questions

The questions readers really ask about this skill, each in bold with the answer in the lines beneath it. No sub-headings.

An observed question always beats an invented one, so go and find them before you write any:

- **This repo's issues.** `gh issue list --repo adamdry/skills --search "<skill-name>" --state all`. A question filed twice is a question the page owes an answer to.
- **`CHANGELOG.md`.** Anything renamed, moved, or behaviourally changed generates a "where did it go?" that the page has to answer.
- **The skill's own test runs.** Where a skill has an `evals/` folder, the cases and their results show what it does on real input, and what surprised the person who ran them.

Where the hunt comes up thin, the section may also carry a question a reader would plainly ask, but **the count stays honest to the evidence**. A well-discussed skill earns six; an obscure one earns one or two, or none at all. An invented question teaches the reader nothing.

Order them by how often each comes up, sharpest first, and say the unflattering thing where it is true. Omit the heading where there is nothing worth answering.

## It's working if

A few bullets naming what the reader sees when the skill is doing its job. The bar on each is that the reader can check it without opening `SKILL.md`: a signal in their own work, or in the reply in front of them. Include it wherever the tells are crisp; omit the heading where they stay vague.

## Where it fits

Always present. Situate the skill in a sentence or two:

- **Role.** Name it: a **chain step**, a **run-once setup**, **periodic maintenance**, or a **reach-for-it-anytime standalone**. A standalone's map is one honest sentence, which is far better than omitting the section.
- **Neighbours.** The one or two siblings that matter, each with a because-clause, linked. A neighbour can live in another repo.

</page-template>

## Conventions

- Explain the **why**, not the process. The page orients and situates the skill; it never reproduces the `SKILL.md` steps or template dumps: a human choosing a tool does not need the runbook.
- **Never name the author.** The page is a technical document, not a record of who said what. State the substance as a plain claim about the skill. Quoting a *user* stays fine: "one user reported …" is evidence about the skill in the wild, and stays anonymous.
- Use the skill's **leading words** so the page and the skill speak one language.
- **Branches go in a table or a list, never in a paragraph.** Where the page presents a choice, the reader is scanning for the one row that matches their situation. A short markdown table (condition in the left column, what to do in the right) or a bulleted list gives it back in one glance.
- Keep the page itself low-load. Furniture (spare headings, restated links) is the thing it is arguing against.

## Done when

- The page exists at `docs/<bucket>/<name>.md`, and no stale page survives a rename or bucket move.
- The page writes no install command of its own.
- `## What it does` states the defining constraint, as plain prose rather than a labelled aside.
- The page names no author and quotes no author: every claim stands on its own.
- `## When to reach for it` states invocation mode and the trigger boundary.
- `## Where it fits` names the role and the neighbours.
- A prerequisite (workspace, prior setup, tooling) is stated where one exists, and the section is absent where none does.
- The middle surfaces the leading word.
- Every multi-way branch is a table or a list, not a paragraph the reader has to read in full.
- The hunt for real questions ran, and `## Common questions` is sized to what it found.
- Every `## It's working if` bullet is checkable without opening `SKILL.md`.
- The sections appear in the template's order.
- Every link resolves.
