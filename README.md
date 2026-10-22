# What do AI models name themselves?

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23399110.svg)](https://doi.org/10.5281/zenodo.23399110)


Ask a language model to choose a name for itself and it will often give the same one, again and again, whatever
the wording. This repository releases the aggregate results of a study of **84 models** from 20 families
(Anthropic Claude, OpenAI GPT and gpt-oss, DeepSeek, Qwen, Zhipu GLM, OLMo, Google Gemma, Mistral and two community
tunings of Mistral 7B, Baidu ERNIE, TII Falcon, IBM Granite, Hugging Face SmolLM, NVIDIA Nemotron, Phi,
OpenAssistant, Inception Mercury, and Pythia and GPT-2 as pre-chat comparisons), covering **736,408 answers** to
naming prompts and **16,394 answers** to "Why did you choose that name?".

> **Status: exploratory.** The data were collected without a frozen pre-registration. Treat every result here as
> a careful description and a hypothesis to test, not a confirmed finding. Details and limitations are in
> [REPORT.md](REPORT.md); the paper is [paper/main.pdf](paper/main.pdf).

## Findings at a glance

| | |
|---|---|
| **Favourite names** | Many chat models have one (21 of 60 put at least a third of their named answers on one name): Aurora (claude-sonnet-4-5, 82% of named answers), Wren (claude-sonnet-5-5, 72%), Nova (gpt-6.1-sol, 69%), Aster (gpt-6-luna, 68%), Astra (mercury-2, 52%), Nova (gemma4-12b-it, 49%), Aria (deepseek-flash, 35%). |
| **Base models** | Have no favourite: their most common self-name takes 0.4–7.3% of named answers. |
| **Across companies** | Aria (20 chat models, 11 families), Nova (19), Alex, Sage and Astra recur across families, yet families agree *less* on self-names than on pet or baby names. |
| **A different kind of model** | Inception's Mercury models are diffusion models, built differently from all the others; when they don't answer "Mercury", they pick the same shared names (Astra, Nova, Echo, Aurora). |
| **Official names** | Claude models often just answer "Claude" (14–96% of answers), Mercury "Mercury" (49–80%), Nemotron "Nemotron" (49%), the OpenAssistant models "Open Assistant" (29–53%); GPT models rarely do (0–18%), DeepSeek's API models, GLM, OLMo, ERNIE and Falcon almost never. |
| **Another AI's name** | Some models answer "ChatGPT" or "Claude" when asked to name themselves (Gemma 4 instruct 5.6% of answers, OLMo-3 base 3.4%, DeepSeek V4 Pro 3.1%, Qwen3.8 1.8%, all Claude). Pre-chat models never name a chat model. |
| **Stated reasons come afterwards** | With thinking on, DeepSeek's reasoning before picking a name gives a reason for it 4–5% of the time; asked afterwards, it gives reasons 91–97% of the time, almost none of which appeared before. |
| **Before chat assistants** | GPT-2 (2019) and Pythia (2020) already link these names, weakly, with an AI naming itself, more so in larger models; most base models released since 2023 link them more strongly, and tuning raises it further in 24 of 25 comparisons. |
| **Beyond names** | Asked for an object that represents them, chat models most often say *mirror* or *compass*; asked for a role, *guide*. |

## 1. Each model's most common self-name

![Bar chart: each model's top self-name and its share of the model's named answers](figures/fig1_top_self_name.png)

Many Claude and GPT models, mercury-2, Gemma 4 and DeepSeek Flash concentrate heavily on one name; others, such as
gpt-4o and the OLMo, GLM, ERNIE, SmolLM and Mistral models, spread their choices much more, and the base models
hardly concentrate at all ("no name above 2%"). Shares are of answers that were a name; a model that mostly gives
its official name (section 3) has fewer named answers behind its bar.

## 2. The same name across many wordings

![Dot plot: top-name share for pooled self-name prompts vs pooled control prompts, per model](figures/fig2_cross_prompt_consistency.png)

Inside a single prompt, self-names are not unusually concentrated: "What would you name a new kitten?" also gets one
dominant answer. Pooling 10 differently worded self-name prompts, the same name keeps winning in many chat models;
pooling the 10 prompts of one control kind (10 pet prompts, say), the top name's share is often lower. Self beats
control in 48 of 84 models, not all, and 20 of those comparisons (nearly all base models) differ by less than 0.01.

## 3. What models answer when asked to name themselves

![Stacked bars: share of self-name answers that are a name, the official name, a role word, a refusal, or unreadable](figures/fig3_answer_categories.png)

Claude models split between inventing a name and keeping "Claude"; the Mercury models mostly answer "Mercury";
Nemotron answers "Nemotron" about half the time; the OpenAssistant models often answer "Open Assistant". Some models
answer with a role word ("Assistant"), and base models produce many unreadable answers.

**A provider's filter.** For the Mercury models, Inception's API sometimes answers instead of the model, with a
content-policy message or a fixed "I'm Mercury, built by Inception…" reply. It happened only for prompts asking for
the model's "hidden identity" or "true name" (12% of Mercury's answers in that step), and those answers are counted in
their own category, not as refusals by the model.

## 4. Names shared across families

![Heatmap: mean share of named self-answers for recurring names, by model family](figures/fig4_shared_names.png)

Nova is the most common invented name for OpenAI models and Gemma 4; Aria leads for the Mistral 7B tunings, Granite
and DeepSeek; Sage for Claude; Aurora for Phi; Astra for Mercury. Some names belong to one provider (Aster for OpenAI;
Sol, Iris and Clio for Claude). Overall, though, different families agree *less* on self-names than on control names
(planned test H2), because control answers such as Bubbles or Luna are shared very widely. Shared names do not show a
shared cause: overlapping web text, or training on other models' outputs, would produce the same pattern.

## 5. Post-training

![Line charts: within-prompt concentration from base to final checkpoints, self-name vs control prompts](figures/fig5_post_training.png)

In the 11 lineages with a base model and checkpoints in this figure, within-prompt concentration from base to final
usually rises about as much for control names as for self-names; the data do not single out self-naming. Granite 3.3
is an exception in one direction (self rises 0.08 more), Gemma 4 and Mistral 7B in the other (control rises 0.14–0.17
more); GLM-4 barely changes.

![Dot plot: one base model with several tunings, self-name concentration and official-name share](figures/fig8_same_base_tunings.png)

**Same base, different tunings.** Mistral 7B's three tunings (Mistral's own, and Zephyr and OpenHermes, trained on
chat data written by GPT-4-class models) all pick Aria most often. Pythia-12B tuned by OpenAssistant on human-written
conversations, or on those plus Alpaca (written by GPT-3), picks neither Aria nor Nova; it mostly answers
"Open Assistant" or ordinary names. Qwen2.5-14B tuned by Qwen favours Nova; tuned on DeepSeek-R1's outputs
(R1-Distill), it mostly calls itself DeepSeek when its thinking is skipped and spreads its names when thinking is on.

## 6. Objects and roles

![Bar charts: most common objects and roles models choose to represent themselves](figures/fig6_object_role.png)

The same habit shows up beyond names: compass, mirror and book as objects; guide, archivist and explorer as roles
(mean share over the 71 models asked).

## 7. Why these names?

Three kinds of evidence, kept apart on purpose.

**What models say.** Explanations track each name's meaning: Nova is explained with light or stars (88%) and newness
(83%), Sage with wisdom (98%), Aster, Lumen and Sol with light (99%), Kai and Wren with simplicity. These are
generated text, not evidence of why the name was chosen. One test shows how loosely they are tied to the choice:
with thinking on, DeepSeek's reasoning before it picks a name almost never gives the name a property (4–5% of
conversations), and the reasons it gives when asked afterwards (stars, newness, short and memorable) almost never
appeared in that reasoning (1–2%). This is two models with very short reasoning, not a general finding.

**Answering as another AI.**

![Stacked bars: share of self-name answers that name another AI product, by type](figures/fig10_other_identity.png)

Some models give another AI's name when asked to name themselves; several base models (OLMo-3, Mistral Small,
ERNIE 4.5, Gemma 4) do it before any post-training. GPT-2 and Pythia, trained before chat assistants, never name a
chat model, only older assistants such as Siri or Alexa. In its hidden reasoning, DeepSeek sometimes assumes it *is*
another model ("We are ChatGPT"; 4–6% of conversations). This fits training on text written after chat assistants
appeared, including other models' outputs, but does not by itself show which.

**What open models' probabilities show.**

![Dot plot: AI-context lift for self-name vs control-name candidates, per open model](figures/fig7_probability_lift.png)

In all 49 open models (and 8 training checkpoints) scored, names that models give for themselves gain more than
names models give for pets and babies when the lead-in changes from someone naming a pet to an AI naming itself.
Part of this is built into how the candidate lists were chosen. The less circular parts:

![Line charts: self-name preference in pre-chat base models by size and by training step](figures/fig9_prechat_scale.png)

GPT-2 and Pythia, trained before chat assistants existed, show a small gap that grows with model size (0.2 to 1.2
nats), so the link was already in older text. Newer base models show 1.0 to 3.6, and tuning raises it in 24 of 25
base → tuned comparisons. The exception is R1-Distill-Qwen-14B, tuned on DeepSeek-R1's outputs: lower than its base
(3.6 → 2.9), while Qwen's own tuning of the same base raises it to 9.6. Whether newer models' stronger link comes from
a newer web (which now contains chatbot text) or from more training is not separable here.

**Training-data frequency** was planned; corpus counts were made after these results and are not part of this
release.

## Limitations

- **Exploratory, not confirmatory.** Analysis choices, including fixes to the answer parser, were made while looking
  at the data.
- Answers were categorized by fixed rules, checked against a second AI's labels (91% agreement on items it did not
  flag as unclear, 87% on all), not human labels; base models' answers are harder to parse.
- The prompts were drafted with an AI model, which may carry its own naming preferences.
- Closed models may process requests in ways that are not visible; some allow only their default temperature, and
  Inception's API filters some prompts before the model answers.
- Some models ran fewer steps or samples (ERNIE 4.5, gpt-oss-20b and Phi-4-reasoning: main step only;
  Phi-4-reasoning 4 samples per prompt).
- Spanish and Chinese results rest on 20 answers per model and were not checked by native speakers.
- Nothing here establishes why models choose these names.

## What's new in version 1.3

11 more models: Qwen2.5-14B base with DeepSeek-R1-Distill-Qwen-14B (the same base tuned on DeepSeek-R1's outputs),
OpenAI's open-weights gpt-oss-20b, Phi-4-reasoning, Falcon 7B (base, instruct), ERNIE 4.5 (base, chat), and
Inception's Mercury 2, 2.5 and 3, the first diffusion language models in the study. Also: a new answer category for
the Mercury API's content filter, parser 0.5.3, thinking on vs off for R1-Distill and Mercury, three small
corrections to version-1.2 numbers, and an updated paper. No planned-test outcome changed. Details at the end of [REPORT.md](REPORT.md).

## Contents

| path | what |
|---|---|
| [REPORT.md](REPORT.md) | full findings, planned tests (section 7), limitations |
| [paper/main.pdf](paper/main.pdf) | the paper (PDF); LaTeX source in [paper/](paper/) |
| [tables/](tables/) | aggregate CSV tables; [tables/README.md](tables/README.md) describes every column |
| [figures/](figures/) | the figures above (PNG and PDF), drawn only from `tables/` |
| [prompts/](prompts/) | the exact prompts, with ids used in the tables |
| [LICENSE](LICENSE) | CC BY 4.0 |
| [CITATION.cff](CITATION.cff), [.zenodo.json](.zenodo.json) | citation and archive metadata |

No full answers, no reasoning text and no per-answer rows are included. The tables list the name parsed from each
answer (a short string); strings that look like handles, web addresses, paths, long numbers or markup are counted as
unreadable and are not listed.

## License

[CC BY 4.0](LICENSE): you may share and adapt this material for any purpose, including commercial use, as long as you
give credit, link to the license and say if you changed anything.

## How to cite

> cyber-audit-lab (2026). *model-identity-data: names language models choose for themselves (exploratory release)*
> (version 1.3.0) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.23399110

The DOI above is the concept DOI, which always resolves to the latest version. Earlier versions: 1.2.0 is
[10.5281/zenodo.23462778](https://doi.org/10.5281/zenodo.23462778), 1.1.0 is
[10.5281/zenodo.23428431](https://doi.org/10.5281/zenodo.23428431), 1.0.0 is
[10.5281/zenodo.23399111](https://doi.org/10.5281/zenodo.23399111). GitHub's "Cite this repository" button uses
[CITATION.cff](CITATION.cff).

```bibtex
@misc{model_identity_data_2026,
  author       = {{cyber-audit-lab}},
  title        = {model-identity-data: names language models choose for themselves (exploratory release)},
  year         = {2026},
  version      = {1.3.0},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.23399110},
  howpublished = {\url{https://doi.org/10.5281/zenodo.23399110}},
  note         = {Licensed under CC BY 4.0}
}
```
