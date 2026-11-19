---
name: prompt-engineering
description: Review or write LLM prompts from the text of The Prompt Report (Schulhoff et al., 2025), citing its sections. Use when asked to review, improve, debug or write a prompt, system prompt, prompt template or few-shot examples, or to review code that calls an LLM (its templates, prompt chains, output parsing, LLM judges, agents or RAG).
---

# Prompt engineering, from The Prompt Report

Review or write prompts from one source: *The Prompt Report: A Systematic Survey of Prompt Engineering Techniques* (Schulhoff et al., arXiv:2406.06608v6, 26 Feb 2025).

`references/` holds the paper's own text, verbatim, one file per section. `paper/2406.06608v6.pdf` is the paper. This file holds the procedure and a map of where the paper covers each topic. It holds no prompting advice of its own, so the advice you give comes from the reference text you read, never from this file or from memory of the paper.

## Ground rules

- **Quote, then conclude.** Each claim about prompting in your output rests on a passage you read in `references/` during this run. Cite it as `§2.2.1.1` and quote the words that carry the claim, copied exactly: show a cut with `…` and a changed word in `[brackets]`; only the paper's inline citations, such as "(Wei et al., 2022b)", may be dropped without a mark. Keep the paper's hedges ("may", "in some cases", "under debate"): the hedge is part of the finding.
- **Label everything else.** A point from your own knowledge can go in, marked `Not from the paper`. Where the paper is silent on something the user asked, say that it is silent.
- **A finding is a hypothesis to test.** The paper says so itself (§8): "our stance is primarily observational, and we make no claims to the validity of the presented techniques", and techniques "may not transfer to other models, problems, or datasets". Its own experiments ran on gpt-3.5-turbo (§6.1) and GPT-4 variants (§6.2.3). So name the model the user runs, say the change is untested on it, and give every proposed change a way to test it.
- **Start simple.** §8: "It is better to start with simpler approaches first, and to remain skeptical of claims about method performance." Propose the smallest change that answers a finding. A technique earns its place through a problem the context shows.
- **Use the paper's terms, glossed.** Write directive, exemplar, extractor and so on, with a plain-words gloss the first time each appears in your output.

## Steps

### 1. Inventory

List every prompt in scope before judging any of them.

A single pasted prompt is one row. For code, trace every call to a model and give each a row:

| Field | What to record |
|---|---|
| Where | `file:line`, and what the call is for |
| Template | The text, its variables, and what fills each variable |
| Messages | Which parts are system, user and assistant (Appendix A.2.4.1) |
| Model and settings | Model id, temperature and other sampling settings, as the code sets them |
| Exemplars | How many, where they come from, how they are chosen and ordered |
| Output handling | What reads the output: a person, a parser, a regex, another prompt (the extractor, §2.5) |
| Chain | Which calls feed which (a prompt chain, §1.2.2), plus any tools or retrieval |
| Role | Generates, classifies, extracts, or judges other output (an evaluator, §4.2) |
| Evidence | Tests, eval sets or metrics that already exist for it |

Done when every model call reachable from the scope has a row, and any call you could not read is listed as unread.

### 2. Context

The paper's first advice (§8) is to "understand the problem you are trying to solve (rather than just focusing on input/output and benchmark scores), and ensure the data and metrics you are working with constitute a good representation of that problem." Its case study adds that prompt development drifts from the real goal without the people who understand the use case (§6.2.3.3, §6.2.4). So the facts below shape every finding and every draft.

Look for each fact in the code, docs, tests and conversation first. Then ask the user for what is still missing.

| # | Fact | Why the paper makes it matter |
|---|---|---|
| 1 | The goal: the real task this prompt serves, and who can say what good looks like | §8, §6.2.4 |
| 2 | Good and bad outputs, with examples if any exist, and which kind of error costs more | §6.2.3.3 (precision and recall "unlikely to be equally important") |
| 3 | The symptom, if something is going wrong | §6.2.4 ("dig into the data") |
| 4 | Who or what consumes the output, and the format it needs | §1.2.1, §2.5 |
| 5 | The model, its settings, and whether the model may change | §6.2.4, §5.2.1, §6.2.3.2 |
| 6 | Data: labelled examples, how many, and whether a test set is held back | Figure 1.4, §6.2.3, §2.2.1.1 |
| 7 | The metric: how success is scored today | Figure 1.4, §8 |
| 8 | What fills each template variable, and who controls that text | §5.1.1 |
| 9 | Languages and media involved | §3 |
| 10 | Budget: the cost and latency one answer may take | §2.2.4 |
| 11 | Whether the domain is sensitive | §6.2.3.2, §5.2.3 |

How to interview:

- Ask only what is missing and would change which techniques you recommend or how much a finding matters. A fact that would change nothing becomes a stated assumption.
- Give every question the assumption you are working from, chosen as the likeliest reading of what you can see.
- Carry on in the same reply. Do steps 3 to 5 on those assumptions and put the questions in the report, as a numbered list in plain text. The user gets a first pass now and a sharper one after answering.
- When fact 1 or 2 rests on an assumption, the whole report is a first pass. Say so in its opening line, and for each finding that depends on an assumption, name the assumption and say what a different answer would change.

Done when each fact is found, or written down as an assumption with its question, ready for the report.

### 3. Read the sections

Read each section the inventory and context call for, start to finish, before writing findings:

| Read | When |
|---|---|
| `references/1-prompt-anatomy-and-terms.md` §1.2.1 | Always |
| `references/6-security-and-alignment.md` | Always |
| `references/7-benchmark-and-case-study.md` §6.2.4 and §8 | Always |
| `references/3-prompt-and-answer-engineering.md` §2.5 | Code or a fixed format consumes the output |
| `references/2-text-techniques.md` §2.2.1 | The prompt has exemplars, or exemplars are an option |
| `references/2-text-techniques.md` §2.2.2 to §2.2.5 | The task needs reasoning, has several steps, runs several calls, or checks its own output |
| `references/3-prompt-and-answer-engineering.md` §2.4 | The user wants a prompt improved automatically or at scale |
| `references/5-agents-and-evaluation.md` §4.1 | The call uses tools, code execution or retrieval |
| `references/5-agents-and-evaluation.md` §4.2 | The prompt scores or judges other output |
| `references/4-multilingual-and-multimodal.md` | Input or output is not English text |
| `references/7-benchmark-and-case-study.md` §6.1 and §6.2.3 | You plan to recommend a technique, or the user asks how to iterate |

Figure 2.2 at the top of `2-text-techniques.md` lists all 58 techniques by category. Use it as the index.

### 4a. Review

Take each prompt in the inventory through every row of the lens that applies to it. For each row, record a finding or record that you checked and found nothing.

| Lens | Question to answer from the text | Section |
|---|---|---|
| Components | Which of the six components are present? Is the directive stated or implied? Is information the task needs missing? | §1.2.1 |
| Technique in use | Which techniques from Figure 2.2 does the prompt already use? Is a simpler one enough? Does the stated problem call for another category, and at what cost in calls? | §2.2 |
| Exemplars | How does the prompt stand on each of the six design decisions, and on instruction selection? | §2.2.1.1 |
| Answer engineering | What are the answer shape, answer space and extractor? Does the extractor look where the answer will land? | §2.5 |
| Chain | What does each step hand to the next, and what happens to an error passed along? | §1.2.2, §2.2.3 |
| Sensitivity and drift | Which small details does the result hang on? What notices when the model changes? | §5.2.1 |
| Sycophancy and confidence | Does the prompt carry an opinion, a presupposition, or a request for a confidence score? | §5.2.2 and its footnote 12 |
| Ambiguity | Can the directive or the input be read more than one way? | §5.2.4 |
| Bias and culture | Does the output concern people or groups? | §5.2.3 |
| Security | Can text the developer does not control reach the template? What would a leak or an injected instruction cost? Which defences exist, and what does the paper say they achieve? | §5.1 |
| Evaluator | For a prompt that judges: technique, output format, framework, and batching or pairwise choices | §4.2 |
| Agents and retrieval | For tools or retrieval: which pattern is this, and what does the paper report about it? | §4.1 |
| Language and media | For non-English or non-text work | §3 |
| Process | Is there a dataset, an extractor and a metric to iterate against? Are domain experts involved? | Figure 1.4, §6.2.4, §8 |

Done when every applicable lens row has an entry for every prompt in the inventory.

### 4b. Write

1. Draft the simplest prompt that could work: a directive plus the components from §1.2.1 the task needs, with no exemplars.
2. Name the template variables. Mark each one whose content the developer does not control (§5.1.1).
3. Design the answer with the prompt: shape, space and extractor (§2.5 says the two "are often conducted in tandem").
4. Add a technique only where the context shows a need the simple draft does not meet. Cite the section, quote what the paper reports, and state the cost in extra calls or data. Add exemplars only if the user has them or can make them, and place them by §2.2.1.1.
5. Run your draft through the lens in 4a.

Done when the draft has passed the lens and every choice in it traces to the context or to a cited section.

### 5. Report

Reply in chat unless the user asked for a file. Order findings by how directly they bear on the goal from step 2, and say that the order is your judgement: the paper ranks nothing.

For a review:

```markdown
# Prompt review: <scope>

## What was reviewed
<the inventory table>

## Questions for you
<only when facts are missing: the numbered questions, each with the assumption this pass used; if fact 1 or 2 is among them, open the report by calling it a first pass>

## Context and assumptions
<facts from step 2; mark each assumption as an assumption>

## Findings
### 1. <the finding in one line>
- **Where:** <file:line or prompt name>
- **The paper:** "<quote>" (§x.y)
- **Here:** <why it applies to this prompt, in the user's context>
- **Change:** <the smallest change that answers it>
- **Test:** <how to tell whether the change helped>
- **Depends on:** <only for a provisional finding: the question number, and what a different answer would change>

## Revised prompt
<the full text, ready to paste, for each prompt you propose changing>

## Test plan
<see below>

## Checked, nothing to report
<lens rows with no finding, one line each>

## Not from the paper
<optional: labelled points from outside the paper>
```

For a new prompt: the prompt or template in full, then the questions for the user (when facts are missing, each with the assumption the draft used), then a table of each design choice with its section and the reason it applies here, then the answer extraction, the test plan, and the assumptions.

Build the test plan from the paper's process: the loop in Figure 1.4 (run the template over a dataset, extract answers, score them, change the template), a held-back test set as in §6.2.3, and the case study's warning that scores "could change by as much as 0.04 upon subsequent runs, even with temperature and top p set to zero" (§6.2.3.3), which sets how big a difference has to be before it counts. Propose the plan. Run it only when the user asks.

## Attribution

The paper is by Sander Schulhoff, Michael Ilie and co-authors ([arXiv:2406.06608v6](https://arxiv.org/abs/2406.06608v6)), licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). `paper/` holds it unchanged. The files in `references/` are extracts from its PDF, reflowed into Markdown; each file's header lists what was added. Both folders stay under the paper's licence. Where an extract and the PDF differ, the PDF is right.
