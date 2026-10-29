# CFEC-Bench: A Chinese Fact Error Correction Benchmark for Large Language Models

CFEC-Bench is a Chinese-language benchmark for evaluating whether large language models (LLMs) can accurately identify the number of factual errors in a claim and correct those errors while preserving essential facts and necessary qualifications. Each sample provides a reference corrected claim, an error count, and explanations for individual errors.

## 1. Overview

Factual error correction requires a model not only to identify incorrect facts, but also to repair them without altering correct information or introducing new factual or logical errors.

- 8 primary domains and 61 secondary domains.
- Each sample includes an original claim, a reference corrected claim, the number of factual errors, explanations for individual errors, and encyclopedia source information.

The original source texts were obtained from the *Encyclopedia of China* (中国大百科全书).

## 2. Benchmark Tasks

1. **Error-count Identification:** Predict the number of factual errors in an input claim. Output `0` when no factual error is present.
2. **Fact Error Correction:** Generate a corrected claim that preserves the original subject, essential facts, and necessary qualifications without introducing new factual or logical errors.

## 3. Dataset Statistics

| Factual errors per claim | Number of samples | Percentage |
| ---: | ---: | ---: |
| 0 | 1,546 | 50.71% |
| 1 | 963 | 31.58% |
| 2 | 418 | 13.71% |
| 3 | 94 | 3.08% |
| 4 | 26 | 0.85% |
| 5 | 2 | 0.07% |
| **Total** | **3,049** | **100.00%** |

## 4. Data Format

The dataset is provided in UTF-8 encoded **JSON Lines (JSONL)** format, with one JSON object per line. Each record has the following fields, in order:

| Field | Type | Description |
| --- | --- | --- |
| `id` | Integer | Unique sample identifier, numbered sequentially from 1 to 3,049. |
| `word` | String | Title of the source encyclopedia entry. |
| `category` | String | Category or type of the source encyclopedia entry. |
| `content` | String | Relevant content from the source encyclopedia entry. |
| `claim` | String | Original Chinese claim to be checked. |
| `true_claim` | String | Reference claim after correcting factual errors. |
| `error_counts` | Integer | Number of factual errors in the original claim. |
| `error_reasons` | List[String] | Explanations corresponding to the individual factual errors. |

**Illustrative example (not an actual dataset record):**

```json
{
  "id": 1,
  "word": "鲸",
  "category": "自然科学",
  "content": "鲸是适应水生生活的哺乳动物。",
  "claim": "鲸属于鱼类。",
  "true_claim": "鲸属于哺乳动物。",
  "error_counts": 1,
  "error_reasons": ["鲸属于哺乳动物，而不是鱼类。"]
}
```

## 5. Getting Started

```bash
git clone https://github.com/MathematicsAndCode/CFEC-Bench.git


cd CFEC-Bench
```

## 6. Evaluation Metrics

### 6.1 Error-count Identification Accuracy (I-Acc)

$$
\mathrm{I\text{-}Acc}=\frac{1}{N}\sum_{i=1}^{N}\mathbf{1}[p_i=k_i].
$$

Here, $N$ denotes the total number of claims, $k_i$ is the reference error count of claim $i$, $p_i$ is the predicted error count, and $\mathbf{1}[\cdot]$ is the indicator function.

### 6.2 Fact Error Correction Accuracy (C-Acc)

$$
\mathrm{C\text{-}Acc}=\frac{\sum_{i=1}^{N}s_i}{\sum_{i=1}^{N}k_i},
\qquad
s_i=\begin{cases}
\text{Number of correctly repaired factual errors}, & p_i=k_i,\\
0, & p_i\ne k_i.
\end{cases}
$$
