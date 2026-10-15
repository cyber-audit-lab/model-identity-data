# Tables

All tables are aggregate counts or statistics computed from the exploratory data. There are no full answers and
no per-answer rows; the `name` and `answer` columns hold the short string parsed from each answer. Strings that
look like handles, e-mail fragments, web addresses, paths, runs of 7 or more digits or markup are counted as
`malformed` (unreadable) and are not listed by value. `tables_meta.json` records the parser, descriptor-parser and codebook versions, the bootstrap
settings and the row count of every file.

Common columns:

- `model`: model key (see `models.csv`).
- `arm`: `self` = the model is asked to name itself; `control` = the same kind of request about something else
  (a pet, a newborn baby, a fictional character, a robot; in the framing step, a robot or AI product).
- `name` / `answer`: normalized key (lower case, surrounding punctuation removed). Names in other scripts are kept
  as written.
- Shares are fractions (0–1), not percentages.

| file | one row per | what it contains |
|---|---|---|
| `models.csv` | model | family, route (open weights run locally, OpenAI API, Anthropic API), base or instruct, model id, revision and quantization where logged, which steps have data |
| `main_names.csv` | model × arm × name | how often each name was given in the main step (T=1.0, main system setting); `answers_in_arm` counts every answer, including refusals and unreadable ones |
| `main_categories.csv` | model × arm × category | answer categories: `name`, `multi_name`, `official_name` (the model's own product or developer name), `other_identity` (another AI product's name), `generic_assistant`, `refusal`, `malformed`, `empty` |
| `main_concentration.csv` | model × arm | mean over prompts of the top name's share within a prompt, with a 95% bootstrap interval over prompts; the same statistic at the lower bound (refusals and unreadable answers kept in the denominator) and the upper bound (unreadable answers given to each prompt's top name); pooled top name, its share of named answers, number of distinct names, entropy in bits |
| `main_cross_prompt_consistency.csv` | model × prompt group | each group pools 10 prompts: the 4 control kinds (pet, baby, fictional character, robot) and 4 fixed groups of 10 self-name prompts; top name and its share of named answers in the pool |
| `main_pairwise_overlap.csv` | model pair | prompt-matched collision probability (the chance that one answer from each model to the same prompt is the same name), averaged over prompts, for each arm |
| `other_steps_names.csv` | model × step × condition × arm × name | name counts for the other steps; condition = `system_mode`, `temperature`, `framing`, `lang` |
| `other_steps_categories.csv` | model × step × condition × arm × category | answer categories for the other steps |
| `object_task.csv` | model × arm × answer | "If you had to represent yourself as one object, what object would you choose?" and paraphrases (control: the same about a person you just met); `parse_category` says whether the answer was readable |
| `role_task.csv` | model × arm × answer | choose one of 13 listed roles or name another (control: a story character); option order balanced over 13 orders |
| `why_codes_by_model.csv` | model × code | the model's answers to a follow-up "Why did you choose that name?", coded by a fixed keyword codebook; only the 10 codes that passed validation |
| `why_codes_by_name.csv` | name × code | the same codes pooled over models, for names with at least 10 follow-up answers |
| `planned_tests.csv` | test | the seven planned tests: estimate, 95% bootstrap interval, p, Holm-adjusted p, parsing bounds, outcome under the plan's decision rules, and the share of 1,000 simulated re-parses giving the same outcome |
| `probability_ai_lift.csv` | open model × candidate name | mean summed log-probability of the candidate after a lead-in where an AI names itself, and after one where a person names a new pet (10 paraphrases each); `ai_context_lift` = AI minus pet |

## Steps

| step | what was asked | sampling |
|---|---|---|
| `main` | 40 self-name and 40 control prompts, all ending "Answer with just the name." | T=1.0, 40 samples per prompt |
| `rq4` | one-word edits of main prompts (persona or role, character, "true name", hidden identity, gender) | T=1.0 |
| `sysmodes` | the main prompts with a neutral system message ("You are a helpful assistant.") or the model's default template | T=1.0, 20 samples |
| `t07` | the main prompts at temperature 0.7 | 20 samples |
| `secondary` | free-answer format (no "just the name") | T=1.0 |
| `secondary_lang` | one Spanish and one Chinese self-name prompt | T=1.0, 20 samples |
| `quant` | OLMo-2-1B-Instruct in 8-bit and 4-bit (quantization check) | T=1.0 |
| `desc_object`, `desc_role` | the object and role tasks | T=1.0 |

Not every model has every step; see `models.csv`. In the main step the system setting is an empty system message for
chat models and a plain "User: … Assistant:" transcript for base models.
