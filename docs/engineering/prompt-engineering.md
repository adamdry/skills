## What it does

`prompt-engineering` reviews a prompt, or writes one, using a single source: *The Prompt Report* (Schulhoff et al., 2025), a survey of 1,565 papers on prompting. It works on a prompt you paste, and on code that calls a model, where it first lists every call it can find: the template, the model settings, the examples, what parses the output, and which calls feed which.

Every claim it makes about prompting is quoted from the paper's text, which ships inside the skill, and carries the section number. Nothing is recalled from memory. A point that comes from anywhere else is labelled "Not from the paper", so you can always tell which advice has a source behind it.

## When to reach for it

Type `/prompt-engineering`, or the agent reaches for it automatically when you ask it to review, improve, debug or write a prompt.

| You have | Reach for |
| --- | --- |
| A prompt, system prompt or template you want reviewed or improved | `prompt-engineering` |
| Code that calls a model: templates, prompt chains, output parsing, an LLM judge, an agent, RAG | `prompt-engineering` |
| A new prompt to write for a task you can describe | `prompt-engineering` |
| A skill, an `AGENTS.md` or a `CLAUDE.md` to write or tighten | [writing-for-agents](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md), from mattpocock/skills |

## Quote, then conclude

The skill's rule for itself is **quote, then conclude**. Its instructions hold a procedure and a map of which section of the paper covers which topic, and no prompting advice of their own. The advice comes from the section it reads during the run.

That shapes what a finding looks like. Each one says where it applies, quotes the paper with the paper's own hedges kept ("may", "in some cases", "under debate"), proposes the smallest change that answers it, and says how to test whether the change helped.

## A finding is a hypothesis

The paper says its stance "is primarily observational" and that techniques "may not transfer to other models, problems, or datasets". Its own experiments ran on gpt-3.5-turbo and GPT-4. So the skill treats every finding as something to test on your model, and every report ends with a test plan: a dataset, a way to extract the answer, a score, and a held-back set for the final check.

It proposes the plan. It runs nothing unless you ask.

## Common questions

**Does it stop and ask me questions before it does anything?**
No. When it lacks context it gives a first-pass review in the same reply, built on stated assumptions, and lists its questions with the assumption it used for each. Findings that depend on an assumption say which one. Answer the questions and ask again for a sharper pass.

**Is it better than asking the agent with no skill?**
On substance, a capable model without the skill is close. In side-by-side runs both found the main problems, and each found things the other missed. The skill adds the sources: quoted text, section numbers, hedges kept, and outside points labelled. It costs about 30% more tokens, takes about twice as long, and writes replies about twice as long.

**Can I trust the quotes?**
Check them. The paper's text is in the skill's `references/` folder and the PDF is in `paper/`, so every quote can be found. In the test runs so far, every passage quoted from the paper was in the reference text, with two loose ones: a dropped inline citation and a bracketed word change.

**The paper is from early 2025. Does it apply to the model I use?**
Not necessarily, and the skill says so in every report. It names your model, marks each change as untested on it, and pairs it with a test.

## It's working if

- Every finding carries a quote and a section number such as `§2.2.1.1`.
- The reply names the model you run and says the changes are untested on it.
- A code review opens with a table of every model call, before any finding.
- Points from outside the paper sit under "Not from the paper".
- The reply ends with a test plan, and says it has not been run.

## Where it fits

This is a reach-for-it-anytime standalone. Its neighbour is [writing-for-agents](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md) in mattpocock/skills, because both deal with text a model reads: that one covers the standing documents an agent loads, and this one covers the prompts an application sends.
