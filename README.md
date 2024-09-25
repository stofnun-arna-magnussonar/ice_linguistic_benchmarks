# Icelandic Linguistic Benchmark for LLMs

This repo contains a benchmarking data set to evaluate LLMs’ grammatical knowledge and linguistic ability for Icelandic.

The published benchmark set contains 1160 items spread over 19 categories that are tested with 5 methods, cf. the table below:

|               **method**               |                                **category**                                |  **sum** |
|:--------------------------------------:|:--------------------------------------------------------------------------:|:--------:|
| Sentence grammaticality check (yes/no)* | Simple bad/good sentences                                                         | 40       |
| Sentence grammaticality check (yes/no)* | Attributive agreement                                                      | 88       |
| Sentence grammaticality check (yes/no)* | Predicate agreement                                                        | 28       |
| Sentence grammaticality check (yes/no)* | Word order                                                                 | 28       |
| Sentence grammaticality check (yes/no)* | Verb agreement                                                             | 28       |
| Sentence grammaticality check (yes/no)* | Subject case                                                               | 28       |
| Sentence grammaticality check (yes/no)* | Island effect sentences                                                    | 80       |
| Sentence grammaticality check (yes/no)* | wh-movement                                                                | 20       |
| Sentence grammaticality check (yes/no)* | Topicalization                                                             | 32       |
| Sentence grammaticality check (yes/no)* | Gapping                                                                    | 120      |
| Sentence grammaticality check (yes/no)* | Reflexivization                                                            | 40       |
| Well-formedness check of compound nouns (yes/no) *  | Word formation in several versions                                         | 280      |
| Fill in the blank                      | Anaphoric reference                                                        | 20       |
| Fill in the blank                      | Coreference resolution with nouns and proper nouns                         | 44       |
| Fill in the blank                      | Wug test (past tense of wug verbs)                                         | 20       |
| Fragment answering                     | Fragment answers                                                           | 40       |
| Question answering                     | Coreference resolution with nouns and proper nouns                         | 44       |
| Question answering                     | Attributive agreement (choose between three options)                       | 30       |
| Question answering                     | Word sense disambiguation (yes/no)                                         | 150      |
|                                        |                                                                            | **1160** |

All of the items in the benchmark were created manually and for the top two method types, marked with an asterisk, we doubled the number of items to ask the inverse question as well: i.e. "Is this sentence grammatically **correct**?" vs. "Is this sentence grammatically **incorrect**?" etc. For the word sense disambiguation task, we consider pairs of sentences that contain the same lexical form and we double the number of items to ask the same question with the order of the sentence pairs reversed.

## Translation tasks

We also include the file `translation_tasks.jsonl`, which contains both Icelandic sentences that should be translated into English and vice-versa, along with their reference translations. For the translation from Icelandic to English, we use [garden path sentences](https://en.wikipedia.org/wiki/Garden-path_sentence), which can be used to check whether the target output has successfully parsed the sentence or not. E.g. For the sentence "Birta Líf og Heimir niðurstöðurnar í næstu viku?", the word 'birta' needs to be read as a verb meaning 'publish' and not as the woman's name 'Birta' in order for a reader to comprehend the sentence. If the name 'Birta' appears in the English translation, the model has not succesfully parsed the sentence.

For translation from English to Icelandic, we include sentences that test 1) gender agreement in the target output (e.g. for the source sentence "María is a good driver", the translation of 'good' should agree with the masculine 'bílstjóri' ('driver'), rather than the feminine 'María' in order to form a grammatical sentence) and 2) anaphoric reference in the target output (e.g. for the source sentence "The child poured the milk into the cup and checked to see whether it had gone sour", 'it' should be translated in the feminine, 'hún', to refer to the milk rather than the cup, which is masculine in Icelandic).

These tasks are not meant as machine translation test sets but can serve as an indicator of a model's NLU performance and grammatical capabilities in producing Icelandic text. The output needs to be manually examined, however, as we do not include scripts for automatic evaluation, and these tasks are therefore kept separate from the tasks in `ice_benchmark_set.jsonl`.

## Data format
The benchmark is published in JSONL-files, where each line is a JSON object, and the data is in three different formats: one custom, one for the [BIG-nench harness](https://github.com/google/BIG-bench) and one for [OpenAI Evals](https://github.com/openai/evals). Note that the translation tasks in `translation_tasks.jsonl` are not included in the harness data. The custom type has the following elements:

- "id": id of the sentence, word etc. Contains a keyword for each category, e.g. `islands_1a`.
- "input": the sentence, word etc. embedded in a prompt with instructions for the task at hand.
- "answer": the answer.

The BIG-bench format has the following elements:
- "input": the sentence, word etc. embedded in a prompt with instructions for the task at hand.
- "target": the answer.

The OpenAI Evals format has the following elements:
- "input": a list with a single dict for each task (we omit the system prompt). 
- "ideal": the answer.
