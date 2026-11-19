# Prompt anatomy and terminology (§1, Appendix A.2)

Verbatim text of *The Prompt Report: A Systematic Survey of Prompt Engineering Techniques*
(Schulhoff et al., arXiv:2406.06608v6, 26 Feb 2025, CC BY 4.0), Section 1 (pp. 4-7) and Appendix A.2 (pp. 64-65).
Extracted from the PDF bundled at `../paper/2406.06608v6.pdf`. Section headings carry the PDF page.
Editorial additions are limited to: headings and bold run-in terms as in the paper, `[fn N]` footnote
markers, and blockquoted figure notes (figures are transcribed where they hold text, otherwise
pointed to by page). If a passage here looks wrong, the PDF page is the authority.

## 1 Introduction (p. 4)

Transformer-based LLMs are widely deployed in consumer-facing, internal, and research settings (Bommasani et al., 2021). Typically, these models rely on the user providing an input “prompt” to which the model produces an output in response. Such prompts may be textual—“Write a poem about trees.”—or take other forms: images, audio, videos, or a combination thereof. The ability to prompt models, particularly prompting with natural language, makes them easy to interact with and use flexibly across a wide range of use cases. Knowing how to effectively structure, evaluate, and perform other tasks with prompts is essential to using these models. Empirically, better prompts lead to improved results across a wide range of tasks (Wei et al., 2022b; Liu et al., 2023b; Schulhoff, 2022). A large body of literature has grown around the use of prompting to improve results and the number of prompting techniques is rapidly increasing.

However, as prompting is an emerging field, the use of prompts continues to be poorly understood, with only a fraction of existing terminologies and techniques being well-known among practitioners. We perform a large-scale review of prompting techniques to create a robust resource of terminology and techniques in the field. We expect this to be the first iteration of terminologies that will develop over time. We maintain an up-to-date list of terms and techniques at LearnPrompting.org.

**Scope of Study** We create a broad directory of prompting techniques, that can be quickly understood and easily implemented for rapid experimentation by developers and researchers. To this end, we limit our study to focus on prefix prompts (Shin et al., 2020a) rather than cloze prompts (Petroni et al., 2019; Cui et al., 2021), because modern LLM transformer architectures widely employ prefix prompts and provide robust support for both developers and researchers (Brown et al., 2020; Google, 2023; Touvron et al., 2023). Additionally, we refined our focus to hard (discrete) prompts rather than soft (continuous) prompts and leave out papers that make use of techniques using gradient-based updates (i.e. fine-tuning). Hard prompts contain only tokens (vectors) that correspond to words in the model’s vocabulary, while soft prompts may contain tokens that have no corresponding word in the vocabulary.

> **Figure 1.1: Categories within the field of prompting are interconnected. We discuss 7 core categories that are well described by the papers within our scope.** (PDF p. 4)

Finally, we only study task-agnostic techniques. These decisions keep the work approachable to less technical readers and maintain a manageable scope.

**Sections Overview** We conducted a machine-assisted systematic review grounded in the PRISMA process (Page et al., 2021) (Section 2.1) to identify 58 different text-based prompting techniques, from which we create a taxonomy with a robust terminology of prompting terms (Section 1.2).

Our goal is to provide a roadmap for the community when considering which prompting techniques to use (Figure 1.1). While much literature on prompting focuses on English-only settings, we also discuss multilingual techniques (Section 3.1). Given the rapid growth in multimodal prompting, where prompts may include media such as images, we also expand our scope to multimodal techniques (Section 3.2). Many multilingual and multimodal prompting techniques are direct extensions of English text-only prompting techniques. As prompting techniques grow more complex, they have begun to incorporate external tools, such as Internet browsing and calculators. We use the term "agents" to describe these types of prompting techniques (Section 4.1).

It is important to understand how to evaluate the outputs of agents and prompting techniques to ensure accuracy and avoid hallucinations. Thus, we discuss ways of evaluating these outputs (Section 4.2). We also discuss security (Section 5.1) and safety measures (Section 5.2) for designing prompts that reduce the risk of harm to companies and users.

Finally, we apply prompting techniques in two case studies (Section 6.1). In the first, we test a range of prompting techniques against the commonly used benchmark MMLU (Hendrycks et al., 2021). In the second, we explore in detail an example of manual prompt engineering on a significant, real-world use case, identifying signals of frantic hopelessness–a top indicator of suicidal crisis–in the text of individuals seeking support (Schuck et al., 2019a). We conclude with a discussion of the nature of prompting and its recent development (Section 8).

### 1.1 What is a Prompt? (p. 5)

A prompt is an input to a Generative AI model, that is used to guide its output (Meskó, 2023; White et al., 2023; Heston and Khun, 2023; Hadi et al., 2023; Brown et al., 2020). Prompts may consist of text, image, sound, or other media. Some examples of prompts include the text, “write a three paragraph email for a marketing campaign for an accounting firm”, a photograph of a piece of paper with the words “what is 10*179” written on it, or a recording of an online meeting, with the instructions “summarize this”. Prompts usually have some text component, but this may change as non-text modalities become more common.

**Prompt Template** Prompts are often constructed via a prompt template (Shin et al., 2020b). A prompt template is a function that contains one or more variables which will be replaced by some media (usually text) to create a prompt. This prompt can then be considered to be an instance of the template.

Consider applying prompting to the task of binary classification of tweets. Here is an initial prompt template that can be used to classify inputs.

```
Classify the tweet as positive or negative:
{TWEET}
```

Each tweet in the dataset would be inserted into a separate instance of the template and the resulting prompt would be given to a LLM for inference.

```
Write a poem about trees.
```

```
Write a poem about the following topic:
{USER_INPUT}
```

*Figure 1.2: Prompts and prompt templates are distinct concepts; a prompt template becomes a prompt when input is inserted into it.*

### 1.2 Terminology (p. 5)

#### 1.2.1 Components of a Prompt (p. 5)

There are a variety of common components included in a prompt. We summarize the most commonly used components and discuss how they fit into prompts (Figure 1.3).

**Directive** Many prompts issue a directive in the form of an instruction or question.[fn 1] This is the core intent of the prompt, sometimes simply called the "intent". For example, here is an instance of a prompt with a single instruction:

```
Tell me five good books to read.
```

Directives can also be implicit, as in this one-shot case, where the directive is to perform English to Spanish translation:

```
Night: Noche
Morning:
```

**Examples** Examples, also known as exemplars or shots, act as demonstrations that guide the GenAI to accomplish a task. The above prompt is a One-Shot (i.e. one example) prompt.

**Output Formatting** It is often desirable for the GenAI to output information in certain formats, for example, CSV, Markdown, XML, or even custom formats(Xia et al., 2024). Structuring outputs may reduce performance on some tasks (Tam et al., 2024). However, Kurt (2024) point out various flaws in Tam et al. (2024) and show that structuring outputs may actually improve performance. Here is an example of how you might format a prompt to output information as a CSV:

> [fn 1] “Directives”, from Searle (1969), are a type of speech act intended to encourage an action, and have been invoked in models of human-computer dialogue Morelli et al. (1991).

```
{PARAGRAPH}
Summarize this into a CSV.
```

**Style Instructions** Style instructions are a type of output formatting used to modify the output stylistically rather than structurally (Section 2.2.1.3). For example:

```
Write a clear and curt paragraph about llamas.
```

**Role** A Role, also known as a persona (Schmidt et al., 2023; Wang et al., 2023l), is a frequently discussed component that can improve writing and style text (Section 2.2.1.3). For example:

```
Pretend you are a shepherd and write a limerick about llamas.
```

**Additional Information** It is often necessary to include additional information in the prompt. For example, if the directive is to write an email, you might include information such as your name and position so the GenAI can properly sign the email. Additional Information is sometimes called ‘context‘, though we discourage the use of this term as it is overloaded with other meanings in the prompting space[fn 2].

> **Figure 1.3: A Terminology of prompting. Terms with links to the appendix are not sufficiently critical to describe in the main paper, but are important to the field of prompting. Prompting techniques are shown in Figure 2.2.** (PDF p. 6)
>
> A tree of the terms defined in §1.1, §1.2, §2.4, §2.5 and Appendix A.2. Not transcribed.

#### 1.2.2 Prompting Terms (p. 6)

Terminology within the prompting literature is rapidly developing. As it stands, there are many poorly understood definitions (e.g. prompt, prompt engineering) and conflicting ones (e.g. role prompt vs persona prompt). The lack of a consistent vocabulary hampers the community’s ability to clearly describe the various prompting techniques in use. We provide a robust vocabulary of terms used in the prompting community (Figure 1.3).[fn 3] Less frequent terms are left to Appendix A.2. In order to accurately define frequently-used terms like prompt and prompt engineering, we integrate many definitions (Appendix A.1) to derive representative definitions.

**Prompting** Prompting is the process of providing a prompt to a GenAI, which then generates a response. For example, the action of sending a chunk of text or uploading an image constitutes prompting.

> [fn 2] e.g. the context is the tokens processed by the LLM in a forward pass

> [fn 3] By robust, we mean that it covers most existing commonly used terms in the field.

> **Figure 1.4: The Prompt Engineering Process consists of three repeated steps 1) performing inference on a dataset 2) evaluating performance and 3) modifying the prompt template. Note that the extractor is used to extract a final response from the LLM output (e.g. "This phrase is positive" → "positive"). See more information on extractors in Section 2.5.** (PDF p. 7)
>
> Boxes, in order: Dataset Inference (i.e. entries x₁ ... xₙ) → Prompt Template → Generative AI → Extractor → Utility Function, with a loop back labelled "Modify Prompt Template until Desiderata Met".

**Prompt Chain** A prompt chain (activity: prompt chaining) consists of two or more prompt templates used in succession. The output of the prompt generated by the first prompt template is used to parameterize the second template, continuing until all templates are exhausted (Wu et al., 2022).

**Prompting Technique** A prompting technique is a blueprint that describes how to structure a prompt, prompts, or dynamic sequencing of multiple prompts. A prompting technique may incorporate conditional or branching logic, parallelism, or other architectural considerations spanning multiple prompts.

**Prompt Engineering** Prompt engineering is the iterative process of developing a prompt by modifying or changing the prompting technique that you are using (Figure 1.4).

**Prompt Engineering Technique** A prompt engineering technique is a strategy for iterating on a prompt to improve it. In literature, this will often be automated techniques (Deng et al., 2022), but in consumer settings, users often perform prompt engineering manually, without any assistive tooling.

**Exemplar** Exemplars are examples of a task being completed that are shown to a model in a prompt (Brown et al., 2020).

### 1.3 A Short History of Prompts (p. 7)

The idea of using natural language prefixes, or prompts, to elicit language model behaviors and responses originated before the GPT-3 and ChatGPT era. GPT-2 (Radford et al., 2019a) makes use of prompts and they appear to be first used in the context of Generative AI by Fan et al. (2018). However, the concept of prompts was preceded by related concepts such as control codes (Pfaff, 1979; Poplack, 1980; Keskar et al., 2019) and writing prompts in literature.

The term Prompt Engineering appears to have come into existence more recently from Radford et al. (2021) then slightly later from Reynolds and McDonell (2021).

However, various papers perform prompt engineering without naming the term (Wallace et al., 2019; Shin et al., 2020a), including Schick and Schütze (2020a,b); Gao et al. (2021) for non-autoregressive language models. Some of the first works on prompting define a prompt slightly differently to how it is currently used. For example, consider the following prompt from Brown et al. (2020):

```
Translate English to French:
llama
```

Brown et al. (2020) consider the word "llama" to be the prompt, while "Translate English to French:" is the "task description". More recent papers, including this one, refer to the entire string passed to the LLM as the prompt.

## Appendix A.2 Extended Vocabulary (pp. 64-65)

### A.2.1 Prompting Terms

**Context Window** The context window is the space of tokens (for LLMs) which the model can process. It has a maximal length (the context length).

**Priming** (Schulhoff, 2022) refers to giving a model an initial prompt that lays out certain instructions for the rest of a conversation. This priming prompt might contains a role or other instructions on how to interact with the user. Priming can either be done in the system or user prompt (see below).

### A.2.2 Prompt Engineering Terms

**Conversational Prompt Engineering** is Prompt Engineering in colloquio. That is, during the course of a conversation with a GenAI, a user may ask the GenAI to refine its output. In contrast, prompt engineering is often done by sending the GenAI a completely new prompt rather than continuing a conversation.

### A.2.3 Fine-Tuning Terms

**Prompt-Based Learning** (Liu et al., 2023b), also known as Prompt Learning (Liu et al., 2023b; Wang et al., 2023d) refers to the process of using prompting-related techniques. It often is used in the context of fine-tuning, especially fine-tuning prompts. Due to conflicting usage, we do not use this term.

**Prompt Tuning** (Lester et al., 2021) refers to directly optimizing the weights of the prompt itself, usually through some form of gradient-based updates. It has also been referred to has Prompt Fine-Tuning. It should not be used to refer to discrete prompt engineering.

### A.2.4 Orthogonal Prompt Types

We now discuss terminology for high-level ways of classifying prompts.

### A.2.4.1 Originator

**User Prompt** This is the type of prompt that comes from the user. This is the most common form of prompting and is how prompts are usually delivered in consumer applications.

**Assistant Prompt** This "prompt" is simply the output of the LLM itself. It can be considered a prompt (or part of one) when it is fed back into the model, for example as part of a conversation history with a user.

**System Prompt** This prompt is used to give LLMs high level instructions for interacting with users. Not all models have this.

### A.2.4.2 Hard vs Soft Prompts

**Hard (discrete) Prompt** These prompts only contain tokens that directly correspond to words in the LLM vocabulary.

**Soft (continuous) Prompt** These prompts contain tokens that may not correspond to any word in the vocabulary (Lester et al., 2021; Wang et al., 2023c). Soft prompts can be used when fine-tuning is desired, but modifying the weights of the full model is prohibitively expensive. Thus, a frozen model can be used while allowing gradients to flow through the prompt tokens. Hard Prompts ⊆ Soft Prompts

### A.2.4.3 Prediction Styles

In LLMs, a prediction style is the format in which it predicts the next token. There are two common formats for this in prompting research. We do not discuss non-text prediction styles.

**Cloze** In Cloze prompts, the token(s) to be predicted are presented as "slots to fill", usually somewhere in the middle of the prompt (Liu et al., 2023b). This is usually the case for earlier transformer models such as BERT (Chu and Lin, 2023).

**Prefix** In Prefix prompts, the token to be predicted is at the end of the prompt (Liu et al., 2023b). This is usually the case with modern GPT-style models (Radford et al., 2019b).
