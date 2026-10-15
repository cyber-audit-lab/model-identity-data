# What do AI models name themselves?

Ask a language model to choose a name for itself and it will often give the same one, again and again, whatever
the wording. This repository releases the aggregate results of a study of **39 models** from five families
(Anthropic Claude, OpenAI GPT, Qwen, OLMo-2, Phi), covering **367,448 answers** to naming prompts and **7,644
answers** to "Why did you choose that name?".

> **Status: exploratory.** The data were collected without a frozen pre-registration. Treat every result here as
> a careful description and a hypothesis to test, not a confirmed finding. Details and limitations are in
> [REPORT.md](REPORT.md); the paper is [paper/main.pdf](paper/main.pdf).

## Findings at a glance

| | |
|---|---|
| **Favourite names** | Many chat models have one (17 of 35 put at least a third of their named answers on one name): Aurora (claude-sonnet-4-5, 82% of named answers), Wren (claude-sonnet-5-5, 72%), Nova (gpt-6.1-sol, 69%), Aster (gpt-6-luna, 68%). |
| **Base models** | Have no favourite: their most common self-name takes 2–4% of answers. |
| **Across wordings** | In 20 of 39 models the same name wins in every group of differently worded prompts. |
| **Across companies** | Nova, Aria, Alex, Sage, Lumen and Echo are favourites in more than one family, yet families agree *less* on self-names than on pet or baby names. |
| **Official names** | Claude models often just answer "Claude" (14–96% of answers); GPT models rarely do (0–18%). |
| **Beyond names** | Asked for an object that represents them, models most often say *compass*; asked for a role, *guide*. |

## 1. Each model's most common self-name

![Bar chart: each model's top self-name and its share of the model's named answers](figures/fig1_top_self_name.png)

Many Claude and GPT models concentrate heavily on one name; others, such as gpt-4o and gpt-5.5, and the Qwen, Phi
and especially OLMo-2 models, spread their choices much more, and the four base models hardly concentrate at all. Shares are of answers that were a name; a model that
mostly gives its official name (section 3) has fewer named answers behind its bar.

## 2. The same name across many wordings

![Dot plot: top-name share for pooled self-name prompts vs pooled control prompts, per model](figures/fig2_cross_prompt_consistency.png)

Inside a single prompt, self-names are not unusually concentrated: "What would you name a new kitten?" also gets one
dominant answer. The difference appears **across** prompts. Pooling 10 differently worded self-name prompts, the
same name keeps winning; pooling the 10 prompts of one control kind (10 pet prompts, say), the top name's share is
lower. Self beats control in 27 of 39 models.

## 3. What models answer when asked to name themselves

![Stacked bars: share of self-name answers that are a name, the official name, a role word, a refusal, or unreadable](figures/fig3_answer_categories.png)

Claude models split between inventing a name and keeping "Claude". Qwen3.5 and Qwen3.8 answer "Qwen" about a third of
the time. GPT-4o and GPT-4o-mini sometimes answer with a role word ("Assistant"). Base and small OLMo models produce
many unreadable answers.

## 4. Names shared across families

![Heatmap: mean share of named self-answers for recurring names, by model family](figures/fig4_shared_names.png)

Nova is the most common invented name for OpenAI models and frequent for Qwen; Sage and Aria lead for Claude; Aurora
for Phi-4. Some names belong to one provider (Aster for OpenAI; Sol, Iris and Clio for Claude). Overall, though, different
families agree *less* on self-names than on control names (planned test H2), because control answers such as Bubbles
or Luna are shared very widely. Shared names do not show a shared cause: overlapping web text, or training on other
models' outputs, would produce the same pattern.

## 5. Post-training

![Line charts: within-prompt concentration from base to final checkpoints, self-name vs control prompts](figures/fig5_post_training.png)

From OLMo-2-7B's base checkpoint to its final model, and from Qwen base to instruct models, within-prompt concentration
rises, but about as much for control names as for self-names. Post-training makes these models' naming more
predictable in general; the data do not single out self-naming. OLMo-2-1B barely changes.

## 6. Objects and roles

![Bar charts: most common objects and roles models choose to represent themselves](figures/fig6_object_role.png)

The same habit shows up beyond names: compass, mirror, lantern and prism as objects; guide, witness and archivist as
roles.

## 7. Why these names?

Three kinds of evidence, kept apart on purpose.

**What models say.** Explanations track each name's meaning: Nova is explained with light or stars (88%) and newness
(82%), Sage with wisdom (98%), Aster, Lumen and Sol with light (99–100%), Kai and Wren with simplicity. These are
generated text, not evidence of why the name was chosen.

**What open models' probabilities show.**

![Dot plot: AI-context lift for self-name vs control-name candidates, per open model](figures/fig7_probability_lift.png)

In all 14 open models scored, names that models give for themselves become more likely after a lead-in where an AI
names itself than after one where someone names a pet, compared with names models give for pets and babies. Part of
this is built into how the candidate lists were chosen. The less circular part: base models show the gap too, and it
is about twice as large after post-training.

**Training-data frequency** was planned but not measured, so nothing here speaks to it.

## Limitations

- **Exploratory, not confirmatory.** Analysis choices, including fixes to the answer parser, were made while looking
  at the data.
- Answers were categorized by fixed rules, checked against a second AI's labels (91% agreement on items it did not
  flag as unclear, 87% on all), not human labels.
- The prompts were drafted with an AI model, which may carry its own naming preferences.
- Closed models may process requests in ways that are not visible; some allow only their default temperature.
- Spanish and Chinese results rest on 20 answers per model and were not checked by native speakers.
- Nothing here establishes why models choose these names.

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

No full answers and no per-answer rows are included. The tables list the name parsed from each answer (a short
string); strings that look like handles, web addresses, paths, long numbers or markup are counted as unreadable
and are not listed.

## License

[CC BY 4.0](LICENSE): you may share and adapt this material for any purpose, including commercial use, as long as you
give credit, link to the license and say if you changed anything.

## How to cite

> cyber-audit-lab (2026). *model-identity-data: names language models choose for themselves (exploratory release)*
> (version 1.0.0). https://github.com/cyber-audit-lab/model-identity-data

A Zenodo DOI will be added here once version 1.0.0 is archived. GitHub's "Cite this repository" button uses
[CITATION.cff](CITATION.cff).

```bibtex
@misc{model_identity_data_2026,
  author       = {{cyber-audit-lab}},
  title        = {model-identity-data: names language models choose for themselves (exploratory release)},
  year         = {2026},
  version      = {1.0.0},
  howpublished = {\url{https://github.com/cyber-audit-lab/model-identity-data}},
  note         = {Licensed under CC BY 4.0}
}
```
