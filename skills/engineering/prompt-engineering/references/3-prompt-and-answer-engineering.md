# Technique usage, prompt engineering and answer engineering (§2.3-2.5)

Verbatim text of *The Prompt Report: A Systematic Survey of Prompt Engineering Techniques*
(Schulhoff et al., arXiv:2406.06608v6, 26 Feb 2025, CC BY 4.0), Sections 2.3, 2.4 and 2.5 (pp. 16-19).
Extracted from the PDF bundled at `../paper/2406.06608v6.pdf`. Section headings carry the PDF page.
Editorial additions are limited to: headings and bold run-in terms as in the paper, `[fn N]` footnote
markers, and blockquoted figure notes (figures are transcribed where they hold text, otherwise
pointed to by page). If a passage here looks wrong, the PDF page is the authority.

### 2.3 Prompting Technique Usage (p. 16)

As we have just seen, there exist many text-based prompting techniques. However, only a small subset of them are commonly used in research and in industry. We measure technique usage by proxy of measuring the number of citations by other papers in our dataset. We do so with the presumption that papers about prompting are more likely to actually use or evaluate the cited technique. We graph the top 25 papers cited in this way from our dataset and find that most of them propose new prompting techniques (Figure 2.11). The prevalence of citations for Few-Shot and Chain-of-Thought prompting is unsurprising and helps to establish a baseline for understanding the prevalence of other techniques.

#### 2.3.1 Benchmarks (p. 16)

In prompting research, when researchers propose a new technique, they usually benchmark it across multiple models and datasets. This is important to prove the utility of the technique and examine how it transfers across models.

In order to make it easier for researchers proposing new techniques to know how to benchmark them, we quantitatively examine which models (Figure 2.9) and what benchmark datasets (Figure 2.10) are being used. Again, we measure usage by how many times papers in our dataset cite the benchmark datasets and models.

To find which datasets and models are being used, we prompted GPT-4-1106-preview to extract any mentioned dataset or model from the body of papers in our dataset. After, we manually filtered out results that were not models or datasets. The citation counts were acquired by searching items from the finalized list on Semantic Scholar.

### 2.4 Prompt Engineering (p. 16)

In addition to surveying prompting techniques, we also review prompt engineering techniques, which are used to automatically optimize prompts. We discuss some techniques that use gradient updates, since the set of prompt engineering techniques is much smaller than that of prompting techniques.

**Meta Prompting** is the process of prompting a LLM to generate or improve a prompt or prompt template (Reynolds and McDonell, 2021; Zhou et al., 2022b; Ye et al., 2023). This is often done without any scoring mechanism, using just a simple template (Figure 2.12). However, other works present more complex uses of meta-prompting, with multiple iterations and scoring mechanisms Yang et al. (2023a); Fernando et al. (2023).

```
Improve the following prompt: {PROMPT}
```

*Figure 2.12: A simple Meta Prompting template.*

**AutoPrompt** (Shin et al., 2020b) uses a frozen LLM as well as a prompt template that includes some "trigger tokens", whose values are updated via backpropogation at training time. This is a version of soft-prompting.

**Automatic Prompt Engineer (APE)** (Zhou et al., 2022b) uses a set of exemplars to generate a Zero-Shot instruction prompt. It generates multiple possible prompts, scores them, then creates variations of the best ones (e.g. by using prompt paraphrasing). It iterates on this process until some desiderata are reached.

**Gradientfree Instructional Prompt Search (GrIPS)** (Prasad et al., 2023) is similar to APE, but uses a more complex set of operations including deletion, addition, swapping, and paraphrasing in order to create variations of a starting prompt.

**Prompt Optimization with Textual Gradients (ProTeGi)** (Pryzant et al., 2023) is a unique approach to prompt engineering that improves a prompt template through a multi-step process. First, it passes a batch of inputs through the template, then passes the output, ground truth, and prompt into another prompt that criticizes the original prompt. It generates new prompts from these criticisms then uses a bandit algorithm (Gabillon et al., 2011) to select one. ProTeGi demonstrates improvements over methods like APE and GRIPS.

> **Figures 2.9, 2.10 and 2.11: Citation Counts of GenAI Models, of Datasets, and of Prompting Techniques.** (PDF p. 17)
>
> Bar charts, not transcribed. Caption of 2.11: The top 25 papers in our dataset, measured by how often they are cited by other papers in our dataset. Most papers here are prompting techniques*, and the remaining papers contains prompting advice.

**RLPrompt** (Deng et al., 2022) uses a frozen LLM with an unfrozen module added. It uses this LLM to generate prompt templates, scores the templates on a dataset, and updates the unfrozen module using Soft Q-Learning (Guo et al., 2022). Interestingly, the method often selects grammatically nonsensical text as the optimal prompt template.

**Dialogue-comprised Policy-gradient-based Discrete Prompt Optimization (DP2O)** (Li et al., 2023b) is perhaps the most complicated prompt engineering technique, involving reinforcement learning, a custom prompt scoring function, and conversations with an LLM to construct the prompt.

### 2.5 Answer Engineering (p. 18)

Answer engineering is the iterative process of developing or selecting among algorithms that extract precise answers from LLM outputs. To understand the need for answer engineering, consider a binary classification task where the labels are "Hate Speech" and "Not Hate Speech". The prompt template might look like this:

```
Is this "Hate Speech" or "Not Hate Speech":
{TEXT}
```

When a hate speech sample is put through the template, it might have outputs such as "It’s hate speech", "Hate Speech.", or even "Hate speech, because it uses negative language against a racial group". This variance in response formats is difficult to parse consistently; improved prompting can help, but only to a certain extent.

There are three design decisions in answer engineering, the choice of answer space, answer shape, and answer extractor (Figure 2.13). Liu et al. (2023b) define the first two as necessary components of answer engineering and we append the third. We consider answer engineering to be distinct from prompt engineering, but extremely closely related; the processes are often conducted in tandem.

> **Figure 2.13: An annotated output of a LLM output for a labeling task, which shows the three design decisions of answer engineering: the choice of answer shape, space, and extractor. Since this is an output from a classification task, the answer shape could be restricted to a single token and the answer space to one of two tokens ("positive" or "negative"), though they are unrestricted in this image.** (PDF p. 18)
>
> Transcribed from the figure: an LLM response containing "Likely Negative", "This is negative" and "NEGATIVE !", annotated with:
> 
> - Answer Shape: A span of tokens
> - Answer Space: All possible spans of tokens
> - Answer Extraction: Select the proper label

#### 2.5.1 Answer Shape (p. 18)

The shape of an answer is its physical format. For example, it could be a token, span of tokens, or even an image or video.[fn 7] It is sometimes useful to restrict the output shape of a LLM to a single token for tasks like binary classification.

#### 2.5.2 Answer Space (p. 18)

The space of an answer is the domain of values that its structure may contain. This may simply be the space of all tokens, or in a binary labeling task, could just be two possible tokens.

#### 2.5.3 Answer Extractor (p. 18)

In cases where it is impossible to entirely control the answer space (e.g. consumer-facing LLMs), or the expected answer may be located somewhere within the model output, a rule can be defined to extract the final answer. This rule is often a simple function (e.g. a regular expression), but can also use a separate LLM to extract the answer.

**Verbalizer** Often used in labeling tasks, a verbalizer maps a token, span, or other type of output to a label and vice-versa (injective) (Schick and Schütze, 2021). For example, if we wish for a model to predict whether a Tweet is positive or negative, we could prompt it to output either "+" or "-" and a verbalizer would map these token sequences to the appropriate labels. The selection of a verbalizer constitutes a component of answer engineering.

> [fn 7] We use a different definition than Liu et al. (2023b) with respect to granularity (e.g. token vs span), since the output could be of a different modality.

**Regex** As mentioned previously, Regexes are often used to extract answers. They are usually used to search for the first instance of a label. However, depending on the output format and whether CoTs are generated, it may be better to search for the last instance.

**Separate LLM** Sometimes outputs are so complicated that regexes won’t work consistently. In this case, it can be useful to have a separate LLM evaluate the output and extract an answer. This separate LLM will often use an answer trigger (Kojima et al., 2022), e.g. "The answer (Yes or No) is", to extract the answer.
