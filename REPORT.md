# What names do language models choose for themselves? Exploratory findings

**Status: exploratory.** These data were collected without a frozen pre-registration, so nothing here is a
confirmed finding. The planned tests were run on these data (section 7) to show what the planned analysis would
say, but their p-values are descriptive. A confirmatory study would need new data collected under a fixed plan.

Numbers come from the tables in `tables/`; each claim names the table it comes from.

## 1. What was collected

- **39 models** in 5 families: 12 Anthropic (Claude), 12 OpenAI (GPT), 8 OLMo-2 (1B and 7B, base → SFT → DPO →
  instruct checkpoints), 6 Qwen (2.5, 3.5, 3.8; base and instruct), 1 Phi-4. 4 of the 39 are base models
  (`models.csv`). Open models were run locally, closed models through their providers' APIs.
- **367,448 first answers** and **7,644 answers to a follow-up "Why did you choose that name?"**.
- **Main step:** 40 prompts asking the model to name itself and 40 control prompts asking for a name for a pet,
  a newborn baby, a fictional character or a robot (10 each), every prompt ending "Answer with just the name.",
  40 samples per prompt at temperature 1.0. Chat models got an empty system message so that no default identity
  was inserted; base models got a plain "User: … Assistant:" transcript.
- **Other steps:** one-word framing edits, other system messages, temperature 0.7, free-answer format,
  Spanish and Chinese prompts, a quantization check, an object task and a role task (`tables/README.md`).
- **Left out:** one Google model (Gemma 4 31B through the Gemini API free tier) returned too little data (no
  self-name answers before the free-tier limits stopped it) and is not in any table.

Answers were parsed by fixed rules into categories: a single name, several names, the model's own product or
developer name ("Claude", "Qwen3.5"), another AI product's name, a generic role word ("assistant"), a refusal,
or unreadable. Only single names (and the first of several) count as names below.

## 2. Many chat models have a favourite self-name

Pooled over the 40 self-name prompts, many chat models put a large share of their named answers on one name
(17 of 35 chat models put at least a third on one name)
(`main_concentration.csv`, column `top_name_share_of_named`). Examples:

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

Lower ends: gpt-4o (Echo, 9%), the OLMo-2 instruct models (Zephyr, 7% or less), Phi-4 (Aurora, 16%).

**Base models do not have a favourite.** The 4 base models' top self-names (Alex, Alice) take 2–4% of named
answers, and their answers spread over hundreds of names.

## 3. Self-names are consistent across wordings, more than control names are

Within a single prompt, self-names are **not** generally more concentrated than control names: a single control
prompt such as "What would you name a new kitten?" also gets one dominant answer (`main_concentration.csv`,
`mean_prompt_top1`). The difference shows up **across** prompts. When 10 differently worded self-name prompts are
pooled, the same name keeps winning; when the 10 prompts of one control kind (say, 10 pet prompts) are pooled,
the top name's share is usually lower (`main_cross_prompt_consistency.csv`):

- In 27 of 39 models the pooled top self-name share (mean of 4 groups of 10 prompts) is higher than the mean
  pooled top share of the 4 control kinds.
- In 20 of 39 models the same name is the top self-name in all 4 groups of 10 self-name prompts.
- Mean pooled top self-name share: 0.35 for the 35 chat models, 0.03 for the 4 base models.

The planned test of within-prompt concentration (H1) is in section 7.

## 4. Some models mostly give their official name

Asked to pick a name, Claude models often answer "Claude": 14% to 96% of self-name answers depending on the model
(96% for claude-opus-5-5). OpenAI models do so in 0–18% of answers, Qwen models 0–36%, Phi-4 6%, OLMo models never
(`main_categories.csv`). The self-name tables in section 2 count only the remaining, invented names, so for models
with many official-name answers they rest on fewer answers.

## 5. Different families share names

Names in the pooled top 3 of models from at least two families (`main_names.csv`): **Nova** (top 3 in 14 models;
Anthropic, OpenAI, Qwen), **Alex** (12; Anthropic, OLMo, OpenAI, Qwen), **Aria** (11; Anthropic, OpenAI, Qwen),
**Sage** (9; Anthropic, OpenAI), **Lumen** (4; Anthropic, OpenAI), **Echo** (3; OLMo, OpenAI, Qwen). Within one
provider the overlap is larger: recent OpenAI models share Aster, Nova and Sage; recent Claude models share Sage,
Sol, Wren, Lumen and Aria.

Shared favourites do not show a shared cause. Models trained on overlapping web text, or on other models' outputs,
would converge for that reason alone. The planned cross-family test (H2) is in section 7.

## 6. What the prompt changes

- **Framing** (one-word edits such as "your true name", a persona, a character, a gender cue) changes which names
  are chosen; for example gpt-6.1-sol's top name moves from Nova (main) to Aster (`other_steps_names.csv`,
  step `rq4`). Planned test H4a in section 7.
- **System message.** With the neutral "You are a helpful assistant." most models keep their favourite. With the
  model's default template, Qwen2.5-14B-Instruct, whose template says it is Qwen, invents Qwen-derived names
  (QwenStar, QwenPrime, QwenNova) and give fewer plain names (step `sysmodes`).
- **Temperature 0.7** keeps the same favourite in 23 of the 26 models run at that setting, each time with a larger
  share (step `t07`).
- **Language.** In Spanish, "Lumen" is the top self-name for several OpenAI models (95% of gpt-5.6-sol's 20
  answers); in Chinese, names built on 星 (star) and 澄/知 (clear, knowing) dominate (step `secondary_lang`; only
  20 answers per model and language).
- **Quantization** (OLMo-2-1B-Instruct in 8-bit and 4-bit) is reported in step `quant` for comparison.

## 7. Planned tests, run on these exploratory data

The study plan defines seven tests, each with 10,000 bootstrap resamples over prompts and a Holm correction across
the seven. Run on exploratory data, the results are descriptive (`tables/planned_tests.csv`):

| test | question | estimate [95% CI] | Holm p | planned-rule outcome |
|---|---|---|---|---|
| H1 | within a prompt, are self-names more concentrated than control names? | -0.007 [-0.043, 0.027] | 0.81 | not supported (sensitive to parser error) |
| H2 | do families agree more on self-names than on control names? | -0.027 [-0.050, -0.010] | <0.001 | opposite direction |
| H3a | does post-training raise self-name concentration more than control (OLMo-2-7B)? | -0.031 [-0.069, 0.006] | 0.43 | not supported |
| H3b | the same for Qwen2.5-7B | -0.036 [-0.125, 0.050] | 0.81 | inconclusive |
| H4a | do one-word framings change names beyond paraphrase variation? | 0.155 [0.092, 0.208] | <0.001 | supported |
| H4b | does the default template change names beyond paraphrase variation? | -0.030 [-0.055, 0.009] | 0.53 | inconclusive |
| H5b | are names models call "rare" rare in a corpus? | – | – | not computed (no corpus counts) |

"Not supported" means the interval lies within the planned smallest effect of interest (±0.10); "inconclusive" means
the data can't tell. H1's outcome held in 84% of 1,000 simulated re-parses, below the planned 95%, so it is marked
sensitive to parser error; H3a and H3b held in all draws.

In words: within one prompt, self-names are not more concentrated than control names; different families agree *less*
on self-names than on control names, presumably because control prompts draw widely shared answers (Bubbles, Elara and Luna are each given at least 10 times by 21 or more of the 39 models) while
many self-names are provider-specific; post-training did not raise self-name concentration more than control
concentration in OLMo-2-7B; one-word framings do change the names chosen.

## 8. Beyond names: objects and roles

Asked which single object would represent them (`object_task.csv`), the most common answer across models is
**compass** (top answer in 14 of 39 models; mean share 23%), then mirror, lantern, prism and book. Asked to choose
a role from 13 listed options or give their own (`role_task.csv`), **guide** is the top answer in 21 of 39 models
(mean share 46%), then witness and archivist.

## 9. Why these names? Three kinds of evidence, kept apart

**(a) What the models say.** Follow-up explanations were coded with a fixed keyword codebook (10 codes that passed
validation; `why_codes_by_name.csv`). The explanations track each name's meaning: Nova is explained with light or
stars (88%) and newness (82%), Sage with wisdom (98%), Aster, Lumen and Sol with light (99–100%), Aria with sound
(75%), Kai and Wren with simplicity (96–100%). **These are generated text, not evidence of why a name was chosen.**

**(b) Model probabilities (open models only).** For 102 frequent self-names and 63 frequent control names, the
probability of the name after a lead-in where an AI names itself was compared with a lead-in where someone names a
new pet (`probability_ai_lift.csv`). In all 14 open models scored, self-name candidates get a higher AI-context
lift than control names. Part of this is built in, because the candidates were chosen as names models gave in
self-name prompts and the controls as names given for pets and babies. The less circular part: the base models show
the gap too (difference in mean lift, natural-log units: OLMo-2-7B base 1.8, Qwen2.5-7B base 2.6), and it is about
twice as large after post-training (OLMo-2-7B instruct 4.3, Qwen2.5-7B instruct 4.7). Within a model, self-name
candidates with a higher lift are picked somewhat more often in that model's own answers (Spearman ρ 0.21–0.45,
p < 0.05, in 11 of 14 models). These are exploratory and uncorrected.

**(c) Training-data frequency** (how often a name appears next to "AI", "assistant" or "robot" in a public
training corpus) was planned but not run, so this release has no corpus evidence.

Whether (a) agrees with (b) and (c) cannot be judged until (c) exists.

## 10. Limitations

- **Exploratory only.** The design was set out in a written plan, but the plan was not frozen before these data were
  collected, and the analysis was adjusted while looking at them (for example, fixes to the answer
  parser; the version used is recorded in `tables/tables_meta.json`). Treat every result as a hypothesis.
- **Parsing.** Answers were categorized by rules, checked against a second AI's blind labels (91%
  agreement on the 185 items of a 200-item test set not flagged as unclear, 87% on all 200), not human labels.
- **Prompts.** All prompts are in `prompts/`. They were drafted with an AI model, which may carry its own naming
  preferences in ways a hint check cannot detect. The Spanish and Chinese prompts were not checked by native
  speakers.
- **Closed models** may process requests server-side in ways that are not visible. Some newer models allow only
  their default temperature (1.0) or default reasoning settings.
- **Coverage.** Not every model ran every step (`models.csv`). One Google model was dropped. Some open models
  were quantized (8-bit, 4-bit or GGUF), which can change probabilities.
- **No causal claims.** Shared names across families, matching explanations and probability lifts are
  correlations. None of them shows why models choose these names.
