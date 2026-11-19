# Skills

My agent skills. Each one is small, easy to adapt, and built on a source you can check.

Hack around with them. Make them your own.

## Installation

A plugin is a read-only bundle you subscribe to. [skills.sh](https://skills.sh) copies editable files into your project, and you update them by hand. Pick one per agent, because installing both gives you every skill twice.

### 1. Get the skills

<details>
<summary><strong>Claude Code</strong></summary>

```bash
claude plugin marketplace add adamdry/skills
claude plugin install adamdry-skills@adamdry
```

To update, run `claude plugin marketplace update adamdry`, then `claude plugin update adamdry-skills@adamdry`.

</details>

<details>
<summary><strong>Any other agent, or editable files</strong> (manual updates)</summary>

```bash
npx skills@latest add adamdry/skills
```

To update, run `npx skills@latest update`, and re-run `add` to pick up new skills.

</details>

### 2. You're ready to go.

## Why These Skills Exist

### #1: The Agent Reviewed My Prompt From Memory

> "We encourage the reader to avoid taking any claims at face value and to recognize that techniques may not transfer to other models, problems, or datasets."
>
> Schulhoff et al., [The Prompt Report](https://arxiv.org/abs/2406.06608)

**The Problem**. Ask an agent to review a prompt and you get confident advice with no source. Some of it rests on research. Some of it is folklore. You can't tell which is which, and neither can the agent.

**The Fix** is [`/prompt-engineering`](./skills/engineering/prompt-engineering/SKILL.md). It reviews or writes prompts from the text of one paper, The Prompt Report, a survey of 1,565 papers on prompting. The paper's own words ship with the skill. Every finding quotes them, cites the section, and comes with a way to test it on your model.

It also reads code. Point it at a file that calls a model and it lists every call first: the template, the settings, the examples, and what parses the output.

## Reference

These split on one axis: who can invoke them. **User-invoked** skills are reachable only when you type them. **Model-invoked** skills can be invoked by you _or_ reached for automatically by the agent when the task fits.

### Engineering

Skills for code work.

**Model-invoked**

- **[prompt-engineering](./skills/engineering/prompt-engineering/SKILL.md)**: Review or write LLM prompts from the text of The Prompt Report, citing its sections. Also reviews code that calls a model: its templates, prompt chains, output parsing, LLM judges, agents and RAG.

## Licence

The skills are MIT licensed. The paper text and PDF inside `prompt-engineering` belong to the paper's authors and are CC BY 4.0. [`CREDITS.md`](./CREDITS.md) has the details.
