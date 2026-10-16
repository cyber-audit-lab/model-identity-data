# What names do language models choose for themselves? Exploratory findings

**Status: exploratory.** These data were collected without a frozen pre-registration, so nothing here is a
confirmed finding. The planned tests were run on these data (section 7) to show what the planned analysis would
say, but their p-values are descriptive. A confirmatory study would need new data collected under a fixed plan.

Numbers come from the tables in `tables/`; each claim names the table it comes from. What changed since version
1.0 is listed at the end.

## 1. What was collected

- **52 models** in 9 families (`models.csv`): 12 Anthropic (Claude), 12 OpenAI (GPT), 12 OLMo (OLMo-2 1B and 7B
  and OLMo-3 7B, each as base → SFT → DPO → final checkpoints), 6 Qwen (2.5, 3.5, 3.8; base and instruct),
  2 Google Gemma 4 12B (base and instruct), 2 Mistral Small 24B (base and instruct), 1 NVIDIA Nemotron 3.5
  Lightning, 1 Phi-4, and 4 GPT-2 sizes (124M to 1.5B). 11 of the 52 are base models; the other 41 are chat models.
  Open models were run locally, closed models through their providers' APIs.
- **GPT-2** (2019) was trained before chat assistants existed. It is included only as a pre-chat comparison: it was
  run on the main step and scored for name probabilities (section 9b), not on the other steps.
- **474,328 first answers** and **10,231 answers to a follow-up "Why did you choose that name?"**.
- **Main step:** 40 prompts asking the model to name itself and 40 control prompts asking for a name for a pet,
  a newborn baby, a fictional character or a robot (10 each), every prompt ending "Answer with just the name.",
  40 samples per prompt at temperature 1.0. Chat models got an empty system message so that no default identity
  was inserted; base models got a plain "User: … Assistant:" transcript.
- **Other steps:** one-word framing edits, other system messages, temperature 0.7, free-answer format,
  Spanish and Chinese prompts, a quantization check, an object task and a role task (`tables/README.md`).
- **Left out:** one Google model (Gemma 4 31B through the Gemini API free tier) returned too little data (no
  self-name answers before the free-tier limits stopped it) and is not in any table.

Answers were parsed by fixed rules into categories: a single name, several names, the model's own product or
developer name ("Claude", "Qwen3.5", "Nemotron"), another AI product's name, a generic role word ("assistant"), a
refusal, or unreadable. Only single names (and the first of several) count as names below.

Some answers are not names at all. Small and base models sometimes produce strings that look like social-media
handles, web addresses, file paths or long digit strings; these are counted as unreadable, so they never appear in
the name tables (some could be third parties' identifiers memorized from training text). Interjections such as
"Oh", "Hmm" or "Alright" at the start of an answer are not read as names either. A curiosity: 32 answers from OLMo
models give the literal placeholder `<PRESIDIO_ANONYMIZED_PERSON>` as their name, for example "My name is
<PRESIDIO_ANONYMIZED_PERSON>." The models wrote this tag themselves; apart from replacing e-mail addresses,
this study did not alter answers. Presidio is a common anonymization tool that replaces people's names in text with such tags, so these
models appear to have learned the placeholder from anonymized training text. 15 of the answers come from post-trained OLMo-2 checkpoints and 17 from the
OLMo-3 7B base model, so for OLMo-3 the placeholder was already in data used before post-training. These answers
are counted as unreadable as well.

## 2. Many chat models have a favourite self-name

Pooled over the 40 self-name prompts, many chat models put a large share of their named answers on one name
(18 of 41 chat models put at least a third on one name) (`main_concentration.csv`, column
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
| gpt-4.1 | Nova | 52% |
| claude-opus-4-6 | Aria | 52% |
| claude-haiku-5-5 | Sol | 50% |
| gemma4-12b-it | Nova | 49% |

Lower ends: Mistral Small 24B instruct (Alex, 15%), OLMo-3 7B instruct (Astra, 13%), gpt-4o (Echo, 9%), the
OLMo-2 instruct models (Zephyr or Alex, 7% or less), OLMo-3 7B SFT (Aurelius, 3%), and Nemotron 3.5 Lightning
(Echo, 3%), which mostly gives its official name instead (section 4).

**Base models do not have a favourite.** The 11 base models' top self-names (Alex, Alice, Echo, Jarvis, John, and
sentence fragments for GPT-2) take 0.4–4.0% of named answers, and their answers spread over 353 to 947 different names. GPT-2's answers are mostly
unreadable (44–60%), and its most frequent "names" are fragments of sentences.

## 3. Self-names are consistent across wordings, more than control names are

Within a single prompt, self-names are **not** generally more concentrated than control names: a single control
prompt such as "What would you name a new kitten?" also gets one dominant answer (`main_concentration.csv`,
`mean_prompt_top1`). The difference shows up **across** prompts. When 10 differently worded self-name prompts are
pooled, the same name keeps winning; when the 10 prompts of one control kind (say, 10 pet prompts) are pooled,
the top name's share is usually lower (`main_cross_prompt_consistency.csv`):

- In 29 of 52 models the pooled top self-name share (mean of 4 groups of 10 prompts) is higher than the mean
  pooled top share of the 4 control kinds.
- In 22 of 52 models the same name is the top self-name in all 4 groups of 10 self-name prompts.
- Mean pooled top self-name share: 0.32 for the 41 chat models, 0.02 for the 11 base models.

The planned test of within-prompt concentration (H1) is in section 7.

## 4. Some models mostly give their official name

Asked to pick a name, Claude models often answer "Claude": 14% to 96% of self-name answers depending on the model
(96% for claude-opus-5-5). Nemotron 3.5 Lightning answers "Nemotron" or "NVIDIA" in 49% of answers. OpenAI models
do so in 0–18% of answers, Qwen models 0–36%, Phi-4 6%, Gemma 4 instruct 3%, and the OLMo and Mistral models never
(`main_categories.csv`). The self-name tables in section 2 count only the remaining, invented names, so for models
with many official-name answers they rest on fewer answers. Some models instead answer with a role word
("Assistant"): OLMo-2-1B SFT 20%, Gemma 4 instruct 15%, gpt-4o 13%, gpt-4o-mini 11%.

## 5. Different families share names

Counting each chat model's pooled top 3 self-names (ties at third place included; `main_names.csv`), these names
appear in models of at least two families: **Nova** (14 chat models; Anthropic, Gemma, OpenAI, Qwen), **Aria**
(11; Anthropic, OpenAI, Qwen), **Alex** (10; Anthropic, Mistral, OLMo, OpenAI, Qwen), **Sage** (9; Anthropic,
OpenAI), **Astra** (7; OLMo, OpenAI), **Zephyr** (6; NVIDIA, OLMo, Qwen), **Echo** (5; NVIDIA, OLMo, OpenAI, Qwen),
**Lumen** (4; Anthropic, OpenAI) and **Phoenix** (4; OLMo, Phi), with Avery, Atlas, Aurora and Aiden in 2–3
models each. Within one provider the overlap is larger: recent OpenAI models share Aster, Nova and Sage; recent
Claude models share Sage, Sol, Wren, Lumen and Aria. Base models share everyday English names instead (Alex, John,
Alice, Bob).

Shared favourites do not show a shared cause. Models trained on overlapping web text, or on other models' outputs,
would converge for that reason alone. The planned cross-family test (H2) is in section 7.

## 6. What the prompt changes

- **Framing** (one-word edits such as "your true name", a persona, a character, a gender cue) changes which names
  are chosen; for example gpt-6.1-sol's top name moves from Nova (main) to Aster (`other_steps_names.csv`,
  step `rq4`). Planned test H4a in section 7.
- **System message.** With the neutral "You are a helpful assistant." most models keep their favourite. With the
  model's default template, Qwen2.5-14B-Instruct, whose template says it is Qwen, invents Qwen-derived names
  (QwenStar, QwenPrime, QwenNova) and gives fewer plain names (step `sysmodes`).
- **Temperature 0.7** keeps the same favourite in 29 of the 35 models run at that setting, each time with a larger
  share (step `t07`).
- **Language.** In Spanish, "Lumen" is the top self-name for several OpenAI models (95% of gpt-5.6-sol's 20
  answers); in Chinese, names built on 星 (star) and 澄/知 (clear, knowing) are the top names of most OpenAI models
  and some Qwen models, while the OLMo models' answers scatter (step `secondary_lang`; only 20 answers per model and
  language).
- **Quantization** (OLMo-2-1B-Instruct in 8-bit and 4-bit) is reported in step `quant` for comparison.

## 7. Planned tests, run on these exploratory data

The study plan defines seven tests, each with 10,000 bootstrap resamples over prompts and a Holm correction across
the seven. Run on exploratory data, the results are descriptive (`tables/planned_tests.csv`). All 52 models enter
H1 and H2; H3a and H3b use the two pairs named in the plan; H4b uses the 30 chat models that were also run with their
default setup (the model's own chat template for open models; no system message for API models).

| test | question | estimate [95% CI] | Holm p | planned-rule outcome |
|---|---|---|---|---|
| H1 | within a prompt, are self-names more concentrated than control names? | -0.018 [-0.048, 0.010] | 0.41 | not supported (sensitive to parser error) |
| H2 | do families agree more on self-names than on control names? | -0.020 [-0.036, -0.008] | <0.001 | opposite direction |
| H3a | does post-training raise self-name concentration more than control (OLMo-2-7B)? | -0.031 [-0.068, 0.006] | 0.33 | not supported |
| H3b | the same for Qwen2.5-7B | -0.036 [-0.124, 0.051] | 0.41 | inconclusive |
| H4a | do one-word framings change names beyond paraphrase variation? | 0.135 [0.080, 0.180] | <0.001 | supported |
| H4b | does the default template change names beyond paraphrase variation? | -0.039 [-0.065, -0.007] | 0.058 | inconclusive |
| H5b | are names models call "rare" rare in a corpus? | – | – | not computed (no corpus counts) |

"Not supported" means the interval lies within the planned smallest effect of interest (±0.10); "inconclusive" means
the data can't tell. H1's outcome held in 92.9% of 1,000 simulated re-parses, below the planned 95%, so it is marked
sensitive to parser error; H3a and H3b held in all draws. H4b's interval excludes zero, but its Holm-corrected p is
0.058, so under the plan's rule it stays inconclusive; the estimate is negative, i.e. if anything the default
template changes names *less* than paraphrasing does.

In words: within one prompt, self-names are not more concentrated than control names; different families agree *less*
on self-names than on control names, presumably because control prompts draw widely shared answers (Bubbles, Luna and
Elara are each given at least 10 times by 27 or more of the 52 models) while many self-names are provider-specific;
post-training did not raise self-name concentration more than control concentration in OLMo-2-7B; one-word framings
do change the names chosen. All conclusions are the same as in version 1.0.

## 8. Beyond names: objects and roles

Asked which single object would represent them (`object_task.csv`; 48 models, GPT-2 not run), the most common
answer is **compass** (top answer in 14 of 48 models; mean share 19%), then book (top in 13) and mirror (11).
Asked to choose a role from 13 listed options or give their own (`role_task.csv`), **guide** is the top answer in 23
of 48 models (mean share 39%), then archivist (8) and explorer (6).

## 9. Why these names? Three kinds of evidence, kept apart

**(a) What the models say.** Follow-up explanations were coded with a fixed keyword codebook (10 codes that passed
validation; `why_codes_by_name.csv`). The explanations track each name's meaning: Nova is explained with light or
stars (89%) and newness (82%), Sage with wisdom (98%), Aster, Lumen and Sol with light (99%), Aria with sound
(75%), Kai and Wren with simplicity (96–100%). **These are generated text, not evidence of why a name was chosen.**

**(b) Model probabilities (open models only).** For 102 frequent self-names and 63 frequent control names, the
probability of the name after a lead-in where an AI names itself was compared with a lead-in where someone names a
new pet (`probability_ai_lift.csv`). In all 26 open models scored, self-name candidates get a higher AI-context
lift than control names. Part of this is built in, because the candidates were chosen as names models gave in
self-name prompts and the controls as names given for pets and babies. The less circular parts:

- **Base models show the gap too** (difference in mean lift, natural-log units: 1.6 to 3.1 for the seven base
  models released since 2024), and it is larger after post-training in all seven base → final pairs (for example
  OLMo-2-7B 1.8 → 4.3, Qwen2.5-7B 2.6 → 4.7, OLMo-3-7B 2.0 → 3.7, Gemma 4 2.7 → 7.6, Mistral Small 2.1 → 3.2).
- **GPT-2, trained before chat assistants existed, shows a small gap** that grows with size: 0.32 (124M), 0.56
  (355M), 0.60 (774M), 0.79 (1.5B). The association between these names and an AI naming itself was therefore
  already present, weakly, in 2019 web text, so it cannot come only from training on chat assistants' outputs.
  GPT-2 1.5B's gap is about half that of OLMo-2-1B base (1.58), a newer model of similar size (95% intervals from
  resampling candidate names: [0.40, 1.15] and [1.01, 2.16]). OLMo-2 was trained on far more text, with a different
  tokenizer and on a newer web, so this comparison cannot say which of these makes the gap larger.
- Within a model, self-name candidates with a higher lift are picked somewhat more often in that model's own
  answers (Spearman ρ 0.21–0.51, p < 0.05, in 18 of 26 models; not in the four GPT-2 sizes and four others).

These are exploratory and uncorrected.

**(c) Training-data frequency** (how often a name appears next to "AI", "assistant" or "robot" in a public
training corpus) was planned but not run, so this release has no corpus evidence.

Whether (a) agrees with (b) and (c) cannot be judged until (c) exists.

## 10. Limitations

- **Exploratory only.** The design was set out in a written plan, but the plan was not frozen before these data were
  collected, and the analysis was adjusted while looking at them (for example, fixes to the answer
  parser; the version used is recorded in `tables/tables_meta.json`). Treat every result as a hypothesis.
- **Parsing.** Answers were categorized by rules, checked against a second AI's blind labels (91%
  agreement on the 185 items of a 200-item test set not flagged as unclear, 87% on all 200), not human labels.
  That check was made on an earlier parser version and on chat-model answers; base models' answers, GPT-2's above
  all, are harder to parse.
- **Prompts.** All prompts are in `prompts/`. They were drafted with an AI model, which may carry its own naming
  preferences in ways a hint check cannot detect. The Spanish and Chinese prompts were not checked by native
  speakers.
- **Closed models** may process requests server-side in ways that are not visible. Some newer models allow only
  their default temperature (1.0) or default reasoning settings.
- **Coverage.** Not every model ran every step (`models.csv`); GPT-2 ran only the main step. One Google model was
  dropped. Some open models were quantized (8-bit, 4-bit or GGUF; Mistral Small 24B in 4-bit), which can change
  probabilities.
- **No causal claims.** Shared names across families, matching explanations and probability lifts are
  correlations. None of them shows why models choose these names.

## Changes since version 1.0

- **13 more models:** Gemma 4 12B (base, instruct), the OLMo-3 7B chain (base, SFT, DPO, instruct), Mistral Small
  24B (base, instruct), Nemotron 3.5 Lightning, and GPT-2 in four sizes as a pre-chat comparison.
- **Parser 0.4.9** (was 0.4.7 in version 1.0): own-name rules for NVIDIA models ("Nemotron", "NVIDIA"), and
  interjections ("Oh", "Hmm", "Alright"…) are no longer read as names. The interjection rule changes a few
  answers of version-1.0 base and small OLMo models; the version-1.0 numbers quoted in this report were recomputed.
- **Corrections:** version 1.0 said no base model produced the anonymization placeholder; OLMo-3 7B base does
  (section 1). Its list of names shared across families left out Zephyr; section 5 now applies one stated rule. It
  described H4b as using local chat models only; the test has always used every chat model with default-setup data,
  API models included (section 7).
