# DExPhish — Explainable AI for Phishing Webpage Detection

> XLM-RoBERTa embeddings + statistical HTML features + SHAP explainability · Submitted to ICIT 2025

---

## Overview

DExPhish is a phishing detection system that analyzes **actual webpage HTML** — not just URLs — to learn the structural, textual, and visual patterns used by phishing pages. Every prediction comes with **SHAP-based explanations** showing which features triggered the detection, making it suitable for security analyst workflows where interpretability matters.

A companion system, **MExPhish**, extends this to multilingual phishing detection using cross-lingual embeddings.

---

## Why HTML, Not Just URLs

URL-based detection is increasingly unreliable. HTML-based detection captures intent through content:
- **Structural**: Hidden form fields, off-domain action targets, iframe injection
- **Textual**: Brand impersonation, urgency language, login prompts
- **Visual**: CSS mimicry of legitimate pages, favicon mismatches

---

## Architecture

```
Raw Webpage HTML
        │
        ├──► HTML Parser ──► Statistical Features (tag ratios, form analysis,
        │                    external link density, script injection indicators)
        │
        └──► XLM-RoBERTa ──► Contextual Embeddings (768-d)
                     │
             ┌───────▼────────┐
             │  Fusion Layer  │ ← Concatenate statistical + semantic features
             └───────┬────────┘
                     │
              Binary Classifier + SHAP Explainer
```

---

## Results

| Model | Accuracy | F1 | TPR @ 1% FPR |
|---|---|---|---|
| DExPhish (Full) | **97.3%** | **0.971** | **89.2%** |
| HTML Features Only | 93.1% | 0.928 | 76.4% |
| XLM-RoBERTa Only | 95.8% | 0.956 | 83.1% |
| URL Baseline | 88.4% | 0.879 | 61.7% |

Fusion of structural + semantic features outperforms either alone.

---

## SHAP Explainability

```
Base rate: 0.12
+ hidden_form_fields:    +0.31
+ off_domain_action:     +0.28
+ brand_keyword_density: +0.19
+ external_script_count: +0.08
- https_present:         -0.04
─────────────────────────────
Prediction: 0.94 (Phishing)
```

---

## Technical Stack

- **Embeddings**: XLM-RoBERTa (Hugging Face transformers)
- **Explainability**: SHAP (TreeExplainer + DeepExplainer)
- **HTML Parsing**: BeautifulSoup4
- **Classification**: Gradient Boosting on fused feature vector

---

## Research

Submitted to **ICIT 2025**. Addresses the gap in XAI-driven phishing detection for security operations.

---

## License

Apache-2.0
