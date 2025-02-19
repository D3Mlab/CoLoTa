
Thank you for visiting this repository!
This repository contains the resources of our paper: **"CoLoTa: A Dataset for Entity-based Commonsense Reasoning over Long-Tail Knowledge"**.
# Introduction
This repository contains the CoLoTa Dataset and implementations of the baselines. CoLoTa is a dataset consisting of 3,300 queries for Commonsense reasoning over Long-Tail entities that can be used for evaluating LLM hallucinations on long-tail entities, as well as commonsense reasoning capabilities of KGQA methods.


The dataset contains two subsets targetting two different tasks: (i) [**Question Answering** subset](https://github.com/D3Mlab/CoLoTa/blob/main/CoLoTa_qa.json) containing 1650 questions based on [StrategyQA dataset](https://github.com/eladsegal/strategyqa) and (ii) [**Claim Verification** subset](https://github.com/D3Mlab/CoLoTa/blob/main/CoLoTa_cv.json) containing 1650 claims based on [Creak dataset](https://github.com/yasumasaonoe/creak).


## Data Downloading Instructions

You can simply clone the repository or clicking on the [**Question Answering**](https://github.com/D3Mlab/CoLoTa/blob/main/CoLoTa_qa.json)  or  [**Claim Verification**](https://github.com/D3Mlab/CoLoTa/blob/main/CoLoTa_cv.json) tasks and clicking on the *Save link as* option. The dataset can be loaded and viewed by any conventional software tools for loading and viewing .json files, such as the VS Code, Sublime Text, Notepad++, etc.



## Data Format

The format of the dataset is in JSON, where each entry contains a query (a question or a claim), the answer, anchor KG entities mentioned in the query and their respective Wikidata QID, an inference rule, relevant KG triples, reasoning steps and the relevant KG triples to each step, and finally the set of reasoning skills and strategies required to answer the query.
An exemplar entry of the dataset:
```json
{
    "id": "S37",
    "query": "Could you travel from Gujan to Aousserd only by car?",
    "answer": false,
    "KG Entities": {
      "Gujan": "Q5103164",
      "Aousserd": "Q2026640"
    },
    "Inference Rule": "Aousserd must be reachable from Gujan by roads to be able to travel between them by car.",
    "KG Triples": "1- (Gujan, country, Iran), 2- (Iran, continent, Asia), 3- (Aousserd, country, Western Sahara), 4- (Western Sahara, continent, Africa)",
    "Reasoning Steps": [
      {
        "Step": "Gujan is located in Iran.",
        "facts used in this step": "(Gujan, country, Iran)"
      },
      {
        "Step": "Iran is located in Asia.",
        "facts used in this step": "(Iran, continent, Asia)"
      },
      {
        "Step": "Aousserd is located in Western Sahara.",
        "facts used in this step": "(Aousserd, country, Western Sahara)"
      },
      {
        "Step": "Western Sahara is located in Africa.",
        "facts used in this step": "(Western Sahara, continent, Africa)"
      },
      {
        "Step": "Asia and Africa are different continents and not connected by roads, so it is not possible to travel from Gujan to Aousserd only by car."
      }
    ],
    "Reasoning Strategy": [
      "geographical",
      "physical"
    ]
  }
```



## Data Curation Methodology
### Query Selection
To generate CoLoTa queries, we first select questions from StrategyQA and claims from CREAK for which the required factual knowledge for answering them is present in Wikidata or that can be rewritten as such queries by targeting them on new KG entities. 
### Entity substitution
In order to ensure that queries target entities from long-tail knowledge, we replace the original famous entities of the query with entities of the same types that are of considerably less amount of popularity. We use the number of Wikidata triples as a measure of popularity of the entities and perform a Google search to ensure the new entities are in fact less famous than the original ones by comparing the amount of search results.

### Question rewriting
We follow the guidelines and schemes proposed by the ["Would you ask it that way"](https://arxiv.org/pdf/2205.12768.pdf) to improve the naturalness of the NL queries, considering aspects such as grammar (e.g., poor word ordering, non-idiomatic) and form (e.g., quizlike, imperative phrasing), and rewrite more natural formulations of the original queries. We also observe that some queries in Creak and StrategyQA are written with making implicit assumptions that are not necessarily correct, so we correct them in the CoLoTa queries.
## Baseline Methods
To run the baselines, use the following command.
```
python -m baselines.run data/ --dataset_name <QA|CV> --scoring_method <zero shot CoT|few shot CoT> --experiment_name <test> --llm_name <gpt-o1|gpt-4o|gpt-3.5-turbo|gemini|groq-llama> --mode <modified|original>
```
 
