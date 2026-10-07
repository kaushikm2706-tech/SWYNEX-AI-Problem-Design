# SWYNEX-AI-Problem-Design

**SWYNEX Technologies – Artificial Intelligence Internship | Task 1: AI Problem Design**
Intern: Kaushik M | Intern ID: SWX-2026-001365 | Date: 7 Oct 2026

> Status: problem-design stage. This document defines the problem, data, constraints and evaluation plan. It reports **no results**, because nothing has been trained yet.

---

## 1. The AI problem (one sentence)

Automatically classify short **Tanglish** customer messages (Tamil written in English letters, mixed with English) into one of six intents, so a small shop owner can sort WhatsApp messages without reading each one.

Example messages:

| Message | Intent |
|---|---|
| "anna en order enga irukku, 3 days aachu" | order_status |
| "order cancel pannunga please" | cancel_order |
| "payment cut aachu but order confirm aagala" | payment_issue |
| "indha shirt L size irukka?" | product_query |
| "product romba mosam, damaged ah vandhuchu" | complaint |
| "ok thanks" | other |

## 2. Why this problem is narrow and worth solving

- Code-mixed Tamil-English text is how many real customers type, and its spelling is not standardised ("irukku", "iruku", "irukkuu"). Tools trained only on clean English handle it poorly.
- The task is a plain **text classification** problem with 6 labels. It is small enough to finish and evaluate honestly.
- Out of scope: replying to the customer, translation, voice messages, languages other than Tanglish/English.

## 3. User

A single small online-shop owner (clothes, groceries, home food) who receives 50–200 WhatsApp messages a day and has no support staff.

**What the user does with the output:** messages tagged `complaint` or `payment_issue` are shown first, because delay costs the most there.

## 4. Data source

| Item | Plan |
|---|---|
| Source | Self-built dataset. I write and label the messages myself, based on how Tanglish customers typically phrase requests. No real customer chats are used. |
| Size | About 300 messages (50 per class). |
| Format | CSV with two columns: `text`, `label`. |
| Privacy | No names, phone numbers or order IDs. Everything is synthetic or anonymised. |
| Split | 70% train / 15% validation / 15% test, stratified by label. The test set is not touched until the final evaluation. |
| Known weakness | I am the only author and labeller, so the data reflects my own writing style. This is the biggest risk (see section 6). |

## 5. Constraints

- **Tiny data:** about 300 examples, so large models cannot be trained from scratch. Use classical methods first (TF-IDF character n-grams + logistic regression), then optionally compare one pretrained embedding model.
- **Spelling variation:** use character n-grams so "irukku" and "iruku" still match.
- **Compute:** must run on a normal laptop CPU.
- **Latency:** under 1 second per message.
- **Cost:** free tools only.
- **Safety:** no personal data stored.

## 6. Evaluation approach

**Metrics**

- **Macro-F1** on the held-out test set (main metric, because it treats all 6 classes equally).
- **Per-class recall**, with extra attention to `complaint` and `payment_issue`.
- **Confusion matrix** to see which intents get mixed up.

**Baselines (the model must beat these to count as useful)**

1. Majority-class guess (expected macro-F1 near 0.05–0.10 on balanced data).
2. Keyword rules (for example "cancel" → cancel_order).
3. TF-IDF + logistic regression (the main model).

**Success criteria (set before training, so I cannot move the goalposts)**

- Macro-F1 ≥ 0.80 on the test set.
- Recall ≥ 0.85 on `complaint` and `payment_issue`.
- Beats the keyword-rules baseline by at least 10 macro-F1 points.

**Robustness check:** write 30 extra test messages with different spellings and word order that never appear in training. Report the score on these separately.

**Main risk:** if my training and test messages come from the same writer, scores will look better than they would on real customers. I will state this limitation next to every number I report.

## 7. Planned next steps

1. Build and label the CSV dataset.
2. Train the keyword-rule and TF-IDF baselines.
3. Report metrics against the criteria above, including failures.
4. Wrap the best model in a small demo (for later internship tasks).

## 8. Repository structure (planned)

```
SWYNEX-AI-Problem-Design/
├── README.md      <- this document
└── data/          <- dataset will be added after labelling
```
