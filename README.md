# Cross-lingual In-context Learning in Multilingual LLMs

This project how Large Language Models (LLMs) can perform tasks in one language when given examples (in-context learning) in a different language. The project focuses on Natural Language Inference (NLI) and Relation Extraction.

## Overview

The core idea is to test the ability of LLMs to transfer knowledge across languages using few-shot prompting. Different strategies for selecting these few-shot examples are compared.

### LLMs Used:
* **For Natural Language Inference (XNLI task):**
    * `google/flan-t5-large` (780M parameters)
    * `google/flan-t5-xl` (3B parameters)
* **For Relation Extraction (SMiLER task):**
    * `Meta-Llama-3-8B-Instruct`

### Experimental Setup:
* **Few-shot examples:** Generally 4 examples were used for NLI (due to `flan-t5` context limits) and 8 for relation extraction.
* **Generation:** Nucleus sampling was used to guide model outputs towards valid labels.
* All experiments were run with a fixed seed for reproducibility.

## Tasks and Datasets

### 1. Cross-lingual Natural Language Inference (XNLI)
* **Goal:** Determine if a hypothesis is `True` (entailment), `False` (contradiction), or `Unknown` (neutral) based on a premise.
* **Cross-lingual Setup:** Few-shot examples are provided in a source language (e.g., English) and the model is tested on a target language (e.g., French or Russian).
* **Prompting Strategies Compared:**
    * **Random Prompting:** Few-shot examples are chosen randomly.
    * **Task Alignment:** An additional instruction explains how labels translate between the source and target languages.
    * **Semantic Alignment:** Few-shot examples are chosen based on their semantic similarity to the test sample (using multilingual BERT embeddings).
* **Key XNLI Findings:**
    * Larger models (`flan-t5-xl`) performed significantly better.
    * Semantic alignment generally yielded the best results for cross-lingual transfer.
    * Performance was better for language pairs sharing scripts (English-French) compared to those with different scripts (e.g., involving Russian).

### 2. Cross-lingual Relation Extraction (SMiLER)
* **Goal:** Identify the relationship (from a set of 36 possible relations) between two entities in a sentence.
* **LLM:** `Meta-Llama-3-8B-Instruct` was used due to its larger context window, necessary for the extensive list of relations.
* **Instruction:** The prompt explicitly listed all 36 possible relation types.
* **Prompting Strategies Compared:**
    * Random Prompting
    * Semantic Alignment
* **Key SMiLER Findings:**
    * Semantic alignment also generally improved performance over random prompting for this task.
