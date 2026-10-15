# model-identity-data

Which names do AI language models choose when asked to name themselves, how much do different models agree, and
what changes the choice? This repository releases the **aggregate results** of an exploratory study of 39 models
(Claude, GPT, OLMo-2, Qwen, Phi-4): 367,448 answers to naming prompts and 7,644 answers to "Why did you choose
that name?".

**Status: exploratory.** No pre-registration was frozen before collection, so the results are hypotheses, not
confirmed findings. See [REPORT.md](REPORT.md), section 10.

## Main observations

- Most chat models have a **favourite self-name** they return across many differently worded prompts (for
  example Aurora for claude-sonnet-4-5, Wren for claude-sonnet-5-5, Nova for gpt-6.1-sol, Aster for gpt-6-luna).
  **Base models do not**: their answers spread over hundreds of names.
- **Several names recur across companies**: Nova, Aria, Alex, Sage, Lumen and Echo are favourites of models from
  more than one family.
- **Claude models often decline to invent a name** and answer "Claude" (up to 96% of answers for one model).
- **Prompt details matter**: framing words, the system message and the language all shift the choice.
- Models explain their choices mostly through the name's meaning (light, stars, wisdom, sound). These explanations
  are generated text, not evidence of cause.

Details, numbers and limitations: [REPORT.md](REPORT.md).

## Contents

| path | what |
|---|---|
| [REPORT.md](REPORT.md) | findings, planned tests, limitations |
| [tables/](tables/) | aggregate CSV tables; [tables/README.md](tables/README.md) describes every column |
| [prompts/](prompts/) | the exact prompts, with ids used in the tables |
| [LICENSE](LICENSE) | CC BY 4.0 |

No per-answer data or answer text is included.

## License

[CC BY 4.0](LICENSE): you may share and adapt this material for any purpose, including commercial use, as long as
you give credit, link to the license and say if you changed anything.

## How to cite

> cyber-audit-lab (2026). *model-identity-data: names language models choose for themselves (exploratory
> release)*. https://github.com/cyber-audit-lab/model-identity-data

```bibtex
@misc{model_identity_data_2026,
  author       = {{cyber-audit-lab}},
  title        = {model-identity-data: names language models choose for themselves (exploratory release)},
  year         = {2026},
  howpublished = {\url{https://github.com/cyber-audit-lab/model-identity-data}},
  note         = {Licensed under CC BY 4.0}
}
```
