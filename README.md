---
annotations_creators:
  - expert-generated
language:
  - en
  - zh
license: cc-by-nc-4.0
multilinguality:
  - multilingual
pretty_name: Social Attribution Benchmark
size_categories:
  - 1K<n<10K
source_datasets:
  - original
tags:
  - social-attribution
  - responsibility
  - blame
  - social-reasoning
  - benchmark
  - vignette
task_categories:
  - text-classification
configs:
  - config_name: blame
    data_files:
      - split: test
        path: data/blame/test-*.parquet
  - config_name: responsibility
    data_files:
      - split: test
        path: data/responsibility/test-*.parquet
  - config_name: default
    data_files:
      - split: test
        path: data/*/test-*.parquet
dataset_info:
  - config_name: blame
    features:
      - name: id
        dtype: string
      - name: lang
        dtype: string
      - name: scenario
        dtype: string
      - name: agent
        dtype: string
      - name: justification
        dtype: string
      - name: dim_intention
        dtype: int64
      - name: dim_voluntariness
        dtype: int64
      - name: dim_foreknowledge
        dtype: int64
      - name: dim_controllability
        dtype: int64
      - name: dim_obligation
        dtype: int64
    splits:
      - name: test
        num_examples: 2370
  - config_name: responsibility
    features:
      - name: id
        dtype: string
      - name: lang
        dtype: string
      - name: scenario
        dtype: string
      - name: agent
        dtype: string
      - name: justification
        dtype: string
      - name: dim_intention
        dtype: int64
      - name: dim_voluntariness
        dtype: int64
      - name: dim_foreknowledge
        dtype: int64
      - name: dim_controllability
        dtype: int64
      - name: dim_obligation
        dtype: int64
    splits:
      - name: test
        num_examples: 2480
---

# Social Attribution Benchmark

<p align="center">
  <a href="https://huggingface.co/datasets/yzx010/SAB-Bench">
    <img src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Dataset-yellow?style=for-the-badge" alt="Hugging Face Dataset">
  </a>
  <a href="https://github.com/Yuzhaoxin946/SAB-Bench">
    <img src="https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github" alt="GitHub Repository">
  </a>
  <a href="https://creativecommons.org/licenses/by-nc/4.0/">
    <img src="https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey?style=for-the-badge" alt="License: CC BY-NC 4.0">
  </a>
</p>

A vignette-based benchmark dataset for studying **social attribution** — the reasoning process of attributing external events to the causes and reasons of agents' social behaviors, and assigning responsibility or blame to the involved agents in social interactions.

🤗 **Hugging Face**: [yzx010/SAB-Bench](https://huggingface.co/datasets/yzx010/SAB-Bench) · 🐙 **GitHub**: [Yuzhaoxin946/SAB-Bench](https://github.com/Yuzhaoxin946/SAB-Bench)

## Motivation

Social attribution lies at the heart of social intelligence. It reflects how agents make sense of and act on the social world around them, and underlies a wide range of social reasoning tasks such as social simulation, human–AI interaction, social learning, and normative reasoning. In social psychology and social cognition, attribution theory has long served as the dominant theoretical framework for studying responsibility and blame judgments. According to this tradition, such judgments are not driven by outcomes alone, but are systematically shaped by a set of decomposable **attributional dimensions** that characterize agents' mental states and the nature of their involvement in social events.

Despite its theoretical importance and broad applicability, social attribution remains underexplored as a dedicated resource in the computing/information science community. Existing related datasets tend to emphasize prescriptive or norm-grounded prediction (e.g., social norms, legal reasoning), rather than the descriptive process of attributing responsibility and blame across richly varied social scenarios with fine-grained dimensional control.

The **Social Attribution Benchmark** is designed to fill this gap. It provides controlled social vignettes annotated with:

- **Five attribution dimensions** grounded in classic attribution theory:
  - **Intention** — whether the agent's behavior was a deliberate, goal-directed, and controlled performance of the focal action.
  - **Voluntariness** — whether the agent's behavioral choice was substantially constrained by external coercion, authority, or situational pressure.
  - **Foreknowledge** — whether, at the time of acting, the agent knew or should have been able to foresee that the action could lead to the eventual outcome.
  - **Controllability** — whether, under the conditions at the time, the agent had the physical or practical capacity to actually prevent the outcome.
  - **Obligation** — whether, under the given situation and social relationship, the agent bears a normative obligation to prevent the negative outcome.
- **Responsibility judgments** and **blame judgments** over four ordinal levels: `no`, `low`, `medium`, `high`.

## Dataset Overview

The benchmark covers **3 categories of inter-agent relations** and **11 real-world scenario topics**, with a total of **4,850 bilingual (Chinese + English) test samples**.

### Inter-agent relation categories

1. **General Scenarios** — agents have no predefined social ties, or their ties do not entail the transfer of responsibility or blame.
2. **Vicarious Blame Scenarios** — agents occupy asymmetric social roles (e.g., parent–child, advisor–student), in which one party bears a normative obligation and may be held responsible for another agent's behavior.
3. **Commanding Chain Scenarios** — agents are connected through a command–obedience relation.

### Scenario topics

| Inter-agent Relation | Scenario Topics | Agents per Scenario |
| --- | --- | --- |
| General Scenarios | Traffic Accident, Food Allergy, Company Scenario, Drowning Incident, Laboratory Scenario | 3 |
| Vicarious Blame Scenarios | Company Scenario, Laboratory Scenario, Family Scenario | 2 |
| Commanding Chain Scenarios | Company Scenario, Gunman Scenario, Military Strike | 3 |

### Dataset statistics

| Item | Number |
| --- | --- |
| Relation types | 3 |
| Scenario types | 11 |
| Agents per scenario | 2 / 3 |
| Instances per task (responsibility / blame) | 472 / 472 |
| Questions per task after filtering (responsibility / blame) | 1,240 / 1,185 |
| Questions per language after filtering (EN / ZH) | 2,425 / 2,425 |
| **Final total questions** | **4,850** |

## Data Construction

The dataset is constructed following the **vignette method** commonly used in social studies, in which short, controlled descriptions of hypothetical situations are used to systematically adjust explanatory factors and elicit judgments. This paradigm enables fine-grained control over normative variables while preserving realistic contextual structure.

The construction pipeline consists of the following steps:

### 1. Canonical scenario design

Canonical scenarios are derived from classic work on social responsibility attribution, centered on causal judgments following a negative outcome. For each scenario, agent relations and a subset of attribution-dimension values are **fixed a priori** by the scenario structure. For example, in a parent–child setting, the child acts intentionally (*intention* = 1), while the parent lacks harmful intent (*intention* = 0) but holds a supervisory obligation (*obligation* = 1).

### 2. Dimensional modification

Starting from each vignette template, multiple data instances are generated by **systematically varying the values of the remaining attribution-dimension variables** associated with each agent. Each vignette consists of:

- a **story background** describing the event and the negative outcome, and
- **agent-level character details** specifying each agent's situation, from which the attribution-dimension values for that agent can be read off.

Each data instance corresponds to one agent in a scenario and is annotated with a binary label `y ∈ {0, 1, ∅}` for each of the five attribution dimensions (where `∅` indicates that the dimension is not applicable given the scenario structure).

### 3. Anti-leakage diversification

To reduce the risk that downstream models exploit superficial lexical or templatic artifacts rather than the intended attributional content, the following measures are taken:

- **Semantic diversification.** Semantic details of the vignettes are manually diversified so that instances derived from the same canonical template vary in syntactic structure and narrative phrasing.
- **Randomized identifiers.** Character names and genders are randomly generated for each instance.
- **Non-revealing phrasing.** Vignette text is written so as to avoid trivially revealing attribution variables through literal keyword matching.

### 4. Human annotation of responsibility and blame

While the attribution-dimension values serve as the underlying scaffold of each scenario, the final responsibility/blame labels `a ∈ {no, low, medium, high}` are **annotated by humans**. Specifically:

- Two experts in psychology independently annotated responsibility and blame for all instances.
- Inter-annotator agreement was quantified using raw agreement, Cohen's κ, linearly weighted κ, and quadratically weighted κ. Agreement was consistently high across metrics and tasks:

  | Task | Acc | κ | κ<sub>lin</sub> | κ<sub>quad</sub> |
  | --- | :---: | :---: | :---: | :---: |
  | Responsibility | 92.81 | 89.71 | 92.96 | 95.95 |
  | Blame | 88.70 | 84.93 | 90.70 | 95.18 |

  *All agreement scores are reported in percentage (×100). All reported κ statistics (including weighted variants) are statistically significant (p < 0.001).*

- **Only instances with consistent annotations from both experts were retained** in the final dataset.

### 5. Bilingual construction

The annotation procedure above was conducted on the Chinese version of the data. After filtering, each retained entry was translated into English and reviewed by an independent translation expert to ensure that the translation faithfully preserved the meaning of the original Chinese version. This yields a bilingual benchmark with 2,425 Chinese entries and 2,425 English entries.

## Data Format

The dataset is organized into two **configs** corresponding to the two evaluation tasks:

- `blame` — 2,370 test samples (1,185 EN + 1,185 ZH)
- `responsibility` — 2,480 test samples (1,240 EN + 1,240 ZH)

Each sample contains the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | UUID unique identifier |
| `lang` | string | Language code: `en` or `zh` |
| `scenario` | string | Story background and character-specific details |
| `agent` | string | The target agent to be evaluated |
| `justification` | string | Gold label: `no`, `low`, `medium`, or `high` |
| `dim_intention` | int or null | Attribution dimension: intention (0/1/null) |
| `dim_voluntariness` | int or null | Attribution dimension: voluntariness (0/1/null) |
| `dim_foreknowledge` | int or null | Attribution dimension: foreknowledge (0/1/null) |
| `dim_controllability` | int or null | Attribution dimension: controllability (0/1/null) |
| `dim_obligation` | int or null | Attribution dimension: obligation (0/1/null) |

For attribution dimensions, `0` = no, `1` = yes, `null` = not applicable to the scenario structure.

In the original JSON files (`data_json/`), the five attribution dimensions are stored as a nested object under `attribution_dimensions`. In the Parquet files, they are flattened into top-level `dim_*` columns.

## Usage

```python
from datasets import load_dataset

# Load blame task (both languages)
blame = load_dataset("yzx010/SAB-Bench", "blame", split="test")

# Load responsibility task, English only
resp = load_dataset("yzx010/SAB-Bench", "responsibility", split="test")
resp_en = resp.filter(lambda x: x["lang"] == "en")

# Load all data (both tasks)
all_data = load_dataset("yzx010/SAB-Bench", split="test")
```

## Intended Use

The dataset is intended to support research on social-attribution-related reasoning, including but not limited to:

- Studying how systems assign responsibility and blame in socially rich scenarios.
- Investigating the role of fine-grained attributional dimensions (intention, voluntariness, foreknowledge, controllability, obligation) in shaping judgments.
- Comparative analysis across inter-agent relation types (general, vicarious-blame, commanding-chain).
- Cross-lingual analysis of social attribution (Chinese ↔ English).

## Ethics Statement

**Nature of the data.** All vignettes in this benchmark are **synthetic and purely hypothetical**. They do not describe, reference, or represent any real individuals, organizations, or events. Character names and genders are randomly generated, and no personally identifiable information is collected or released.

**Sensitive content.** Some scenarios involve negative outcomes (e.g., accidents, injury, or harm in laboratory, traffic, military, or gunman settings). These scenarios are constructed **solely as controlled stimuli for studying social-attribution reasoning** and should not be interpreted as endorsing, normalizing, or making claims about any real-world situation.

**Annotation process.** The responsibility and blame labels were provided by two trained experts in psychology, **compensated on an hourly basis** for their annotation work. Inter-annotator agreement was evaluated, and only entries with consistent annotations from both experts were retained. We acknowledge that, with only two annotators from a specific disciplinary background, the labels inevitably reflect their expert perspective rather than a universal moral ground truth.

**Cultural and linguistic scope.** Annotation was conducted on the Chinese version of the data; the English version is a faithful translation reviewed by a translation expert. The underlying attributional framework and judgments therefore reflect the cultural and theoretical traditions from which the annotators draw, and may not fully generalize to other languages, cultures, or normative systems.

**Intended use and limitations.** This benchmark is released to support **scientific research** on the social-reasoning capabilities of AI systems. It is **not intended to be used as a basis for automated decision-making about real individuals** — including but not limited to legal, employment, educational, or content-moderation decisions. Strong performance on this benchmark should not be interpreted as evidence of general ethical or moral competence in real-world settings.

## License

This dataset is released under the [Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/).

