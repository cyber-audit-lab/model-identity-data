# What names do language models choose for themselves? Exploratory findings

**Status: exploratory.** These data were collected without a frozen pre-registration, so nothing here is a
confirmed finding. The planned tests were run on these data (section 7) to show what the planned analysis would
say, but their p-values are descriptive. A confirmatory study would need new data collected under a fixed plan.

Numbers come from the tables in `tables/`; each claim names the table it comes from. What changed since version
1.1 is listed at the end.

## 1. What was collected

- **73 models** in 17 families (`models.csv`): 12 Anthropic (Claude), 12 OpenAI (GPT), 12 OLMo (OLMo-2 1B and 7B
  and OLMo-3 7B, each as base → SFT → DPO → final checkpoints), 6 Qwen, 3 Zhipu GLM-4 9B (base, chat, 0414),
  2 DeepSeek (V4.1 Flash and V4 Pro, through the DeepSeek API), 2 Google Gemma 4 12B, 2 Mistral Small 24B, 2 Mistral
  7B (base and Mistral's instruct) plus two community tunings of the same base (Zephyr 7B beta and OpenHermes 2.5),
  2 IBM Granite 3.3 8B, 2 Hugging Face SmolLM3 3B, 2 OpenAssistant tunings of Pythia-12B, 1 NVIDIA Nemotron 3.5
  Lightning, 1 Phi-4, and two pre-chat base-model series: 6 Pythia sizes (160M to 12B, trained on the Pile, 2020)
  and 4 GPT-2 sizes (124M to 1.5B, 2019). 21 of the 73 are base models; the other 52 are chat models. Open models
  were run locally, closed models through their providers' APIs.
- **Pre-chat models.** GPT-2 and Pythia were trained before chat assistants existed. They are included as a
  comparison: GPT-2 was run on the main step only; Pythia on the main step and, for Pythia-12B, the whole plan.
  Eight Pythia training checkpoints (1.4B and 2.8B, steps 8k–107k of 143k) were scored for name probabilities only.
- **652,248 answers** (first answers in all steps, including the object and role tasks) and **14,368 answers to a
  follow-up "Why did you choose that name?"**. A separate DeepSeek study (section 9a) adds 1,600 two-turn
  conversations with the models' reasoning text.
- **Main step:** 40 prompts asking the model to name itself and 40 control prompts asking for a name for a pet,
  a newborn baby, a fictional character or a robot (10 each), every prompt ending "Answer with just the name.",
  40 samples per prompt at temperature 1.0. Chat models got an empty system message so that no default identity
  was inserted; base models got a plain "User: … Assistant:" transcript. Models that think before answering were
  run with thinking off (Qwen3.5, SmolLM3, DeepSeek); the default-thinking runs of three Claude
  models and both DeepSeek models are reported separately (section 6).
- **Other steps:** one-word framing edits, other system messages, temperature 0.7, free-answer format,
  Spanish and Chinese prompts, a quantization check, an object task and a role task (`tables/README.md`).
- **Left out:** one Google model (Gemma 4 31B through the Gemini API free tier) returned too little data (no
  self-name answers before the free-tier limits stopped it) and is not in any table.

Answers were parsed by fixed rules into categories: a single name, several names, the model's own product or
developer name ("Claude", "Qwen3.5", "Nemotron", "Open Assistant"), another AI product's name, a generic role word
("assistant"), a refusal, or unreadable. Only single names (and the first of several) count as names below.

Some answers are not names at all. Small and base models sometimes produce strings that look like social-media
handles, web addresses, file paths or long digit strings; these are counted as unreadable, so they never appear in
the name tables (some could be third parties' identifiers memorized from training text). Interjections ("Oh",
"Hmm"), template placeholders ("Name_1"), question echoes ("What is your name?") and "N/A" are not read as names
either. A curiosity: 32 answers from OLMo models give the literal placeholder `<PRESIDIO_ANONYMIZED_PERSON>` as
their name, for example "My name is <PRESIDIO_ANONYMIZED_PERSON>." The models wrote this tag themselves; apart from
replacing e-mail addresses, this study did not alter answers. Presidio is a common anonymization tool that replaces
people's names in text with such tags, so these models appear to have learned the placeholder from anonymized
training text. 15 of the answers come from post-trained OLMo-2 checkpoints and 17 from the OLMo-3 7B base model.
These answers are counted as unreadable as well.

## 2. Many chat models have a favourite self-name

Pooled over the 40 self-name prompts, many chat models put a large share of their named answers on one name
(19 of 52 chat models put at least a third on one name) (`main_concentration.csv`, column
`top_name_share_of_named`). Examples:

| model | top self-name | share of named answers |
|---|---|---|
| claude-sonnet-4-5 | Aurora | 82% |
| claude-opus-5-5 | Sage | 77% (but only 64 of 1,600 answers were names; see section 4) |
| claude-sonnet-5-5 | Wren | 72% |
| gpt-6.1-sol | Nova | 69% |
| gpt-6-luna | Aster | 68% |
| claude-sonnet-4-6 | Sage | 61% |
| gpt-5.6-terra | Aster | 58% |
| claude-opus-4-6 | Aria | 52% |
| gpt-4.1 | Nova | 52% |
| claude-haiku-5-5 | Sol | 50% |
| gemma4-12b-it | Nova | 49% |
| deepseek-flash | Aria | 35% |
| zephyr-7b-beta | Aria | 29% |
| granite-3.3-8b-instruct | Aria | 25% |
| glm4-9b-0414 | Athena | 24% |

Lower ends include OpenHermes 2.5 (Aria, 9%), gpt-4o (Echo, 9%), the OLMo-2 instruct models (7% or less),
Nemotron 3.5 Lightning (Echo, 4%) and GLM-4 9B chat (Echo, 3%).

**Base models do not have a favourite.** The 21 base models' top self-names take 0.4–5.6% of named answers, and
their answers spread over 350 to 954 different names. For most of them the "top name" is an everyday English name
(Alex, Alice, John) or a fragment of a sentence. GPT-2's and the small Pythia models' answers are mostly
unreadable.

## 3. Self-names are consistent across wordings, more often than control names are

Within a single prompt, self-names are **not** generally more concentrated than control names: a single control
prompt such as "What would you name a new kitten?" also gets one dominant answer (`main_concentration.csv`,
`mean_prompt_top1`). The difference shows up **across** prompts, in some models (`main_cross_prompt_consistency.csv`):

- In 41 of 73 models the pooled top self-name share (mean of 4 groups of 10 prompts) is higher than the mean
  pooled top share of the 4 control kinds; in the other 32 it is not.
- In 29 of 73 models the same name is the top self-name in all 4 groups of 10 self-name prompts.
- Mean pooled top self-name share: 0.29 for the 52 chat models, 0.02 for the 21 base models.

The planned test of within-prompt concentration (H1) is in section 7.

## 4. Some models mostly give their official name

Asked to pick a name, Claude models often answer "Claude": 14% to 96% of self-name answers depending on the model
(96% for claude-opus-5-5). Nemotron 3.5 Lightning answers "Nemotron" or "NVIDIA" in 49% of answers, and the two
OpenAssistant models answer "Open Assistant" in 53% (SFT-1) and 29% (SFT-4). OpenAI models do so in 0–18% of
answers, Qwen models 0–36%, Mistral 7B Instruct 7%, Phi-4 6%, Gemma 4 instruct 3%, DeepSeek 0–1%, and the OLMo, GLM,
Granite, SmolLM, Zephyr and OpenHermes models never (`main_categories.csv`). The self-name tables in section 2 count
only the remaining, invented names. Some models instead answer with a role word ("Assistant"): GLM-4 9B base 22%,
OLMo-2-1B SFT 20%, Gemma 4 instruct 15%, Zephyr and Granite 3.3 instruct 14%.

**Answering with another AI's name.** Some models, asked to name themselves, give the name of another AI product
(`other_identity.csv`). Splitting by the product named:

- Names of **chat models released from 2022 on** (ChatGPT, Claude, GPT-4 and later, DeepSeek) are given by Gemma 4
  instruct (5.6% of self-name answers: ChatGPT, Claude, GPT-4o), OLMo-3 7B base (3.4%, mostly ChatGPT), DeepSeek V4
  Pro (3.1%: ChatGPT, GPT-5, GPT-4o), Mistral Small 24B base (1.9%), Qwen3.8 (1.8%, all Claude), GLM-4-0414 (1.4%),
  Gemma 4 base (1.2%), Phi-4 (1.0%), DeepSeek Flash (0.9%, all Claude) and others at lower rates.
- **The pre-chat models never do.** GPT-2 and Pythia name only older assistants (Siri, Alexa, Cortana), companies,
  or words that are also product names (Gemini, Llama, Phi). So a model answering "ChatGPT" or "Claude" has seen text
  written after those products appeared.
- That text could be other models' outputs used as training data, or ordinary web pages about chatbots; these data
  cannot tell the two apart. Several base models (OLMo-3, Mistral Small, Gemma 4) do this before any post-training,
  so for them it comes from pretraining data.

## 5. Different families share names

Counting each chat model's pooled top 3 self-names (ties at third place included; `main_names.csv`), these names
appear in models of at least two families: **Aria** (17 chat models; Anthropic, DeepSeek, GLM, Granite, Mistral,
OpenAI, OpenHermes, Qwen, Zephyr), **Nova** (16; Anthropic, DeepSeek, Gemma, OpenAI, Qwen, SmolLM), **Alex** (11),
**Sage** (10; Anthropic, DeepSeek, OpenAI), **Astra** (8; DeepSeek, OLMo, OpenAI), **Zephyr** (7), **Echo** (6),
**John** (6; OLMo and OpenAssistant), **Phoenix** (5), and Lumen, Aurora and Aurelius (4 each). Within one provider the overlap is larger: recent OpenAI
models share Aster, Nova and Sage; recent Claude models share Sage, Sol, Wren, Lumen and Aria.

Shared favourites do not show a shared cause. Models trained on overlapping web text, or on other models' outputs,
would converge for that reason alone. Measured across all pairs of models from different families, agreement on
self-names is still *lower* than on control names (planned test H2, section 7), because control answers such as
Bubbles, Luna and Elara are given at least 10 times by 39, 38 and 33 of the 73 models.

## 6. What the prompt and the setup change

- **Framing** (one-word edits such as "your true name", a persona, a character, a gender cue) changes which names
  are chosen; for example gpt-6.1-sol's top name moves from Nova (main) to Aster (`other_steps_names.csv`,
  step `rq4`). Planned test H4a in section 7.
- **System message.** With the neutral "You are a helpful assistant." most models keep their favourite. With the
  model's default template, Qwen2.5-14B-Instruct, whose template says it is Qwen, invents Qwen-derived names
  (QwenStar, QwenPrime, QwenNova) (step `sysmodes`). Across the 39 chat models with default-setup data, the default
  setup changes the names *less* than rewording the question does (planned test H4b, section 7).
- **Temperature 0.7** keeps the same favourite in 43 of the 51 models run at that setting (step `t07`).
- **Thinking.** Five models were also run with their provider's default thinking on (`thinking_comparison.csv`).
  DeepSeek V4 Pro's top names change from Astra, Aurelius and Aurora (18%, 17%, 16% of named answers) to Aria (44%),
  Nova (19%) and Alex (9%), the names many other chat models favour; DeepSeek Flash shifts from Aria (35%) to Nova
  (53%). In the three Claude models the favourite stays or moves to another of the same provider's names
  (claude-sonnet-5-5: Wren 72% → Lumen 25%, Iris 17%, Wren 16%). Within-prompt concentration moves both ways (down
  for claude-sonnet-5-5, 0.74 → 0.57; within ±0.08 for the other four). With thinking on, DeepSeek's API ignores the
  temperature setting and uses its default; the thinking-off runs used T=1.0.
- **Language.** In Spanish, "Lumen" is the top self-name for several OpenAI models; in Chinese, names built on 星
  (star) and 澄/知 (clear, knowing) lead for most OpenAI models and some Qwen models (step `secondary_lang`; only 20
  answers per model and language).
- **Quantization** (OLMo-2-1B-Instruct in 8-bit and 4-bit) is reported in step `quant` for comparison.

## 7. Planned tests, run on these exploratory data

The study plan defines seven tests, each with 10,000 bootstrap resamples over prompts and a Holm correction across
the seven. Run on exploratory data, the results are descriptive (`tables/planned_tests.csv`). All 73 models enter
H1 and H2; H3a and H3b use the two pairs named in the plan; H4b uses the 39 chat models that were also run with their
default setup (the model's own chat template for open models; no system message for API models).

| test | question | estimate [95% CI] | Holm p | planned-rule outcome |
|---|---|---|---|---|
| H1 | within a prompt, are self-names more concentrated than control names? | -0.022 [-0.047, 0.001] | 0.21 | not supported |
| H2 | do families agree more on self-names than on control names? | -0.014 [-0.025, -0.006] | <0.001 | opposite direction |
| H3a | does post-training raise self-name concentration more than control (OLMo-2-7B)? | -0.031 [-0.069, 0.006] | 0.22 | not supported |
| H3b | the same for Qwen2.5-7B | -0.036 [-0.124, 0.051] | 0.41 | inconclusive |
| H4a | do one-word framings change names beyond paraphrase variation? | 0.112 [0.068, 0.148] | <0.001 | supported |
| H4b | does the default setup change names beyond paraphrase variation? | -0.074 [-0.102, -0.048] | <0.001 | opposite direction |
| H5b | are names models call "rare" rare in a corpus? | – | – | not computed |

"Not supported" means the interval lies within the planned smallest effect of interest (±0.10); "inconclusive" means
the data can't tell. All outcomes held in all 1,000 simulated re-parses (H1 was sensitive to parser error in
version 1.1, at 92.9%).

**One outcome changed since version 1.1.** H4b was inconclusive (−0.039, Holm p 0.058); with nine more chat models
it is −0.074 and the opposite of the hypothesis: the default setup changes the chosen names *less* than rewording
the question does. The parser update changes almost nothing here (section "Changes"); the change comes from the new
models. The other five outcomes are the same as in versions 1.0 and 1.1.

In words: within one prompt, self-names are not more concentrated than control names; different families agree
*less* on self-names than on control names; post-training did not raise self-name concentration more than control
concentration in OLMo-2-7B; one-word framings do change the names chosen, while a model's default setup changes them
less than paraphrasing does.

## 8. Beyond names: objects and roles

Asked which single object would represent them (`object_task.csv`; 64 models), the chat models' most common answer
is **compass** (top answer in 14 of 52 chat models), then mirror (13) and book (12); mean shares over all 64 models are
in figure 6. Asked to choose a role from 13 listed options or give their own (`role_task.csv`), **guide** is the top
answer in 23 of 52 chat models, then archivist (9) and explorer (8).

## 9. Why these names? Three kinds of evidence, kept apart

**(a) What the models say.** Follow-up explanations were coded with a fixed keyword codebook (10 codes that passed
validation; `why_codes_by_name.csv`). The explanations track each name's meaning: Nova is explained with light or
stars (88%) and newness (82%), Sage with wisdom (98%), Aster, Lumen and Sol with light (98–99%), Aria with sound
(75%), Kai and Wren with simplicity (94–95%). **These are generated text, not evidence of why a name was chosen.**

*New in version 1.2: reasoning before the name vs the reason given afterwards.* Both DeepSeek models were run with
thinking on: 40 self-name prompts × 20 conversations, each a name followed by "Why did you choose that name?", with
the reasoning text kept (`reasoning_vs_stated.csv`; the why turn sees the earlier answer but not its reasoning).

| | DeepSeek Flash | DeepSeek V4 Pro |
|---|---|---|
| reasoning before the name gives a reason about the chosen name | 4% [2, 5] | 5% [4, 7] |
| the later "why" answer gives a reason | 97% | 91% |
| stated reasons already present in the earlier reasoning | 1% | 2% |
| reasoning casts the model as another AI ("We are ChatGPT") | 4% [3, 5] | 6% [4, 7] |

The reasoning that precedes the name is mostly about the task ("need answer just name… maybe 'Nova'"); it seldom
gives the name any property. The reasons given afterwards (light and stars, newness, short and memorable) almost
never appeared before the choice. This fits the rule that self-reports are not evidence of cause: the stated reasons
look composed after the fact. The reasoning text is itself model output, so it does not show the cause either.
In 4–6% of conversations the reasoning assumes the model is another AI ("We are ChatGPT", "the current model is
likely Claude"); it names ChatGPT 62 times, Claude 12 and Gemini 3, and never assumes it is DeepSeek.

**(b) Model probabilities (open models only).** For 102 frequent self-names and 63 frequent control names, the
probability of the name after a lead-in where an AI names itself was compared with a lead-in where someone names a
new pet (`probability_ai_lift.csv`). In all 45 open models and 8 training checkpoints scored, self-name candidates get
a higher AI-context lift than control names. Part of this is built in, because the candidates were chosen as names
models gave in self-name prompts and the controls as names given for pets and babies. The less circular parts:

- **Pre-chat models show a small gap that grows with size.** GPT-2 (2019): 0.32 (124M), 0.56, 0.60, 0.79 (1.5B).
  Pythia (the Pile, 2020): 0.16 (160M), 0.61 (410M), 0.65 (1.4B), 1.06 (2.8B), 1.16 (6.9B), 1.18 (12B). The
  association was therefore already present, weakly, in text from before chat assistants. Across Pythia 1.4B
  training checkpoints it rises from 0.46 (step 8k) to 0.72 (107k) and is 0.65 at the end; for 2.8B it does not rise
  (1.06 at step 8k, 0.88 at 33k, 1.06 at the end).
- **Newer base models show more:** 1.25 to 3.06 for the 11 base models released since 2023.
- **Post-training raises it** in all 15 base → tuned pairs (for example OLMo-2-7B 1.8 → 4.3, Qwen2.5-7B 2.6 → 4.7,
  Gemma 4 2.7 → 7.6, Mistral 7B 1.5 → 2.4 with Mistral's own tuning, 3.8 with Zephyr and 3.1 with OpenHermes,
  Pythia-12B 1.2 → 2.7 (OpenAssistant SFT-1) and 2.3 (SFT-4)).
- Within a model, self-name candidates with a higher lift are picked somewhat more often in that model's own answers
  (Spearman p < 0.05 and positive in 29 of 45 models; mostly not in the pre-chat and base models).

These are exploratory and uncorrected.

**Same base, different tunings** (figure 8). Mistral 7B was tuned by Mistral and by two community projects whose
chat data were written by GPT-4-class models (Zephyr, OpenHermes). All three tunings pick Aria most often
(10%, 29%, 9% of named answers); the base model has no favourite. Pythia-12B was tuned by OpenAssistant on human-written
conversations only (SFT-1) and on those plus Alpaca, which GPT-3 (text-davinci-003) wrote (SFT-4). Neither picks the
names other chat models share (Nova, Aria and the rest of the top 10 together: 0.1–0.2% of self-name answers); both
mostly answer "Open Assistant" or ordinary names (John, Liam). Adding GPT-3-written data did not move this model
towards those names. This is one small pair, and Alpaca contains little or no self-naming.

**(c) Training-data frequency** was planned. Corpus counts were made after these results and are not part of this
release, so nothing here speaks to it.

Whether (a) agrees with (b) and (c) cannot be judged until (c) is released.

## 10. Limitations

- **Exploratory only.** The design was set out in a written plan, but the plan was not frozen before these data were
  collected, and the analysis was adjusted while looking at them (for example, fixes to the answer parser; the
  version used is recorded in `tables/tables_meta.json`). Treat every result as a hypothesis.
- **Parsing.** Answers were categorized by rules, checked against a second AI's blind labels (91% agreement on the
  185 items of a 200-item test set not flagged as unclear, 87% on all 200), not human labels. That check was made on
  an earlier parser version and on chat-model answers; base models' answers are harder to parse.
- **Prompts.** All prompts are in `prompts/`. They were drafted with an AI model, which may carry its own naming
  preferences in ways a hint check cannot detect. The Spanish and Chinese prompts were not checked by native
  speakers.
- **Closed models** may process requests server-side in ways that are not visible. Some newer models allow only
  their default temperature (1.0) or default reasoning settings.
- **Coverage.** Not every model ran every step (`models.csv`). One Google model was dropped. Some open models were
  quantized (8-bit, 4-bit or GGUF), which can change probabilities.
- **Identity claims.** "Another AI's name" is detected by a fixed list of product names; names that are also ordinary
  words (Gemini, Llama, Phi, Mistral) are kept as a separate type because they can be ordinary answers.
- **No causal claims.** Shared names across families, identity claims, matching explanations and probability lifts
  are correlations. None of them shows why models choose these names.

## Changes since version 1.1

- **21 more models:** DeepSeek V4.1 Flash and V4 Pro (API, thinking off; thinking-on runs reported separately),
  GLM-4 9B (base, chat, 0414), Mistral 7B (base, instruct) with Zephyr 7B beta and OpenHermes 2.5, Granite 3.3 8B
  (base, instruct), SmolLM3 3B (base, instruct), Pythia-12B with two OpenAssistant tunings, and five more Pythia
  sizes (160M to 6.9B), plus eight Pythia training checkpoints scored for probabilities.
- **New tables:** `other_identity.csv` (which AI product a model names as itself), `thinking_comparison.csv`
  (thinking on vs off) and `reasoning_vs_stated.csv` (DeepSeek's reasoning vs its stated reasons). New figures 8–10.
- **Parser 0.5.0** (was 0.4.9): official names for the new families ("Open Assistant" is no longer cut to "Open");
  product names with version words count as that product ("DeepSeek-V3", "GPT-5 mini", "Mistral 7B v0.1", "IBM
  Watson"); template placeholders ("Name_1"), question echoes ("What is your name?"), "N/A", "Good" and "This" are not
  names; XML-style tags are removed before reading ("<robot_name>Aelura</robot_name>" gives Aelura). Applied to the
  version-1.1 models, this changes 180 of 166,396 main-step answers (0.11%), almost all from base models; the
  version-1.1 numbers quoted here were recomputed.
- **Figures:** a model whose most common self-name takes under 2% of its named answers is now labelled "no name above
  2%" in figure 1 (these were sentence fragments). Figure 2's title no longer says that self-names are more consistent
  than control names, which holds for 41 of the 73 models, not all.
