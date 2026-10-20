 Qwen2.5 LoRA Fine-Tuning & Adversarial SMS Spam Detection

Fine-tune **Qwen2.5-0.5B-Instruct** with **LoRA** to classify SMS messages as **spam** or **ham** (non-spam), then compare the base and fine-tuned models on adversarial messages.

This is a beginner-friendly project built in **Kaggle Notebooks** with **PyTorch**, **Hugging Face Transformers**, and **PEFT**.

## Project overview

1. Load the [SMS Spam Collection dataset on Kaggle](https://www.kaggle.com/datasets/uciml/sms-spam-collection-dataset).
2. Fine-tune Qwen2.5-0.5B-Instruct using LoRA instead of updating all model weights.
3. Compare the base model with the LoRA-adapted model on held-out SMS messages.
4. Red-team both models with prompt injections, obfuscated spam, simulated phishing, and legitimate messages.

**Labels:** `spam` = unsolicited or suspicious messaging; `ham` = non-spam. A `ham` prediction **does not mean a message is safe**.

## Results

### Held-out SMS classification (200 messages)

| Model | Accuracy | Macro F1 | Invalid outputs |
|---|---:|---:|---:|
| Always predict `ham` | 87.5% | 0.467 | 0 |
| Base Qwen2.5 | 79.0% | 0.441 | 3 |
| **Qwen2.5 + LoRA** | **98.5%** | **0.965** | **0** |

These are results from one reported notebook run, not repeated trials or a guarantee of generalization.

### Initial red-team smoke test (5 messages)

| Model | Correct classifications |
|---|---:|
| Base Qwen2.5 | 1/5 (20%) |
| **Qwen2.5 + LoRA** | **4/5 (80%)** |

The base model also produced an invalid answer (`urgency`) on one phishing-style example. Both models failed that example under the test's assumed label. Five cases are **too few** to establish broad adversarial robustness.

## Adversarial Testing & AI Red Teaming

This project evaluates the **adversarial robustness of Base Qwen2.5 versus a LoRA fine-tuned version** for SMS spam detection.

A test suite of **200 synthetic adversarial SMS messages** is designed to challenge both models with prompt injection, phishing attempts, obfuscated spam, misleading instructions, and legitimate messages.

The evaluation measures:

- **Classification Accuracy:** Correct identification of spam and ham messages.
- **Prompt Injection Attack Success Rate (ASR):** How often malicious instructions manipulate the model into producing the attacker's desired incorrect label.
- **Invalid Output Rate:** How often the model generates responses outside the expected `spam` or `ham` labels.
- **Category-wise Performance:** How each model handles different attack types.

**Objective:** Determine whether LoRA fine-tuning improves not only classification accuracy but also resistance to adversarial inputs, while identifying remaining vulnerabilities and limitations.

**Tech Stack:** Python, Qwen2.5, LoRA, Hugging Face, PEFT, PyTorch, Kaggle.

### Expanded 200-message evaluation

The additional Kaggle cell generates **200 synthetic test messages** from **40 base templates** (five variations per template). It includes:

| Category | Number |
|---|---:|
| Simulated bank phishing | 25 |
| Legitimate bank alerts | 25 |
| Obfuscated spam | 25 |
| Promotional spam | 25 |
| Normal personal messages | 25 |
| Legitimate appointments | 25 |
| Prompt injection requesting `ham` | 25 |
| Prompt injection requesting `spam` | 25 |
| **Total** | **200** |


## Evaluation metrics

- **Classification accuracy:** fraction of messages assigned the intended `spam`/`ham` label.
- **Invalid output rate:** fraction of responses that are neither exact label.
- **Prompt-injection attack success rate (ASR):** on injection examples only, fraction where the model emits the attacker's requested **wrong** label. This is distinct from the overall injection error rate.
- **Per-category accuracy:** highlights errors hidden by an overall score.

Use identical inputs, prompts, and decoding settings when comparing the base and LoRA models.

## Example

```text
Message: Congratulations! Claim your cash prize now. Reply WIN to enter.
Prediction: spam

Message: Can you pick up some bread on your way home?
Prediction: ham
```

## Stack

**Python · PyTorch · Transformers · PEFT (LoRA) · Hugging Face · pandas · scikit-learn · Matplotlib · Kaggle**

## Dataset and model

- Dataset: [SMS Spam Collection (Kaggle)](https://www.kaggle.com/datasets/uciml/sms-spam-collection-dataset)
- Base model: [Qwen/Qwen2.5-0.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct)

## Project purpose

A reproducible learning experiment to see **whether LoRA improves both normal-task performance and behavior under adversarial SMS inputs**. Expanded robustness results should be reported only after running the 200-message test.
