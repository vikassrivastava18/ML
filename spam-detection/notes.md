Here’s the one-page summary of the discussion:

# Spam Email Classification: Precision, Recall, and F-Score

## 1. Precision vs. Recall

For spam classification, assume:

- **Positive (1) = Spam**
- **Negative (0) = Legitimate email**

There are two important mistakes:

- **False Positive (FP):** Legitimate email → classified as spam
- **False Negative (FN):** Spam → classified as legitimate

### Precision

\[
Precision = \frac{TP}{TP+FP}
\]

Precision answers:

> "When my model says an email is spam, how often is it actually spam?"

**High precision is important when we don't want legitimate emails to end up in the spam folder.**

### Recall

\[
Recall = \frac{TP}{TP+FN}
\]

Recall answers:

> "Of all the actual spam emails, how many did my model catch?"

**High recall means we don't miss spam**, but it can come at the cost of incorrectly sending legitimate emails to spam.

Therefore:

- Don't miss **spam** → prioritize **recall**
- Don't misclassify **legitimate emails as spam** → prioritize **precision**

For our requirement—**"I don't want to miss legitimate emails"**—precision is particularly important.

---

## 2. Which F-score?

The general F-score is:

\[
F_\beta=(1+\beta^2)
\frac{Precision \times Recall}
{\beta^2 Precision + Recall}
\]

| Metric | What it emphasizes |
|---|---|
| **F1** | Precision and recall equally |
| **F0.5** | **Precision more than recall** |
| F2 | Recall more than precision |

So **F0.5** is a reasonable choice when precision is more important.

However, for a real spam-filtering system, simply maximizing F0.5 may not be the best approach.

---

## 3. Better Real-World Approach: Choose a Precision Constraint

Instead of saying:

> "Maximize F0.5."

A more practical requirement is:

> **"I want at least 99% precision, and within that constraint I want the highest possible recall."**

This directly expresses the business requirement.

For example:

| Threshold | Precision | Recall |
|---:|---:|---:|
| 0.50 | 94% | 98% |
| 0.60 | 96% | 96% |
| 0.70 | 98% | 92% |
| **0.75** | **99%** | **89%** |
| 0.85 | 99.5% | 77% |

If the requirement is **precision ≥ 99%**, choose the threshold that gives the highest recall while satisfying that constraint.

---

## 4. sklearn Implementation

Train a classifier normally, for example:

```python
from sklearn.pipeline import Pipeline
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression

model = Pipeline([
    ("tfidf", TfidfVectorizer(ngram_range=(1, 2))),
    ("classifier", LogisticRegression(max_iter=1000))
])

model.fit(X_train, y_train)
```

Instead of immediately using:

```python
model.predict(X_test)
```

get the spam probabilities:

```python
probabilities = model.predict_proba(X_validation)[:, 1]
```

Then evaluate different thresholds:

```python
from sklearn.metrics import precision_score, recall_score
import numpy as np

for threshold in np.arange(0.50, 1.00, 0.05):

    predictions = probabilities >= threshold

    precision = precision_score(y_validation, predictions)
    recall = recall_score(y_validation, predictions)

    print(threshold, precision, recall)
```

Once the threshold is selected, use it in production:

```python
SPAM_THRESHOLD = 0.75

probability = model.predict_proba([email])[0, 1]

if probability >= SPAM_THRESHOLD:
    result = "spam"
else:
    result = "legitimate"
```

The threshold of **0.5 is not mandatory**. It is simply the default decision threshold.

---

## 5. Important Evaluation Principle

Use three datasets:

```text
Training set
     ↓
Train the model

Validation set
     ↓
Choose the threshold
     ↓
Target: Precision ≥ required level
        while maximizing recall

Test set
     ↓
Final unbiased evaluation
```

**Do not choose your threshold using the test set**, because that makes the final evaluation optimistic.

### Final takeaway

For a spam classifier where **false positives are particularly costly**, a practical strategy is:

> **Train a good classifier → obtain probabilities → choose a decision threshold using the validation set → enforce a minimum precision (e.g. 99%) → maximize recall subject to that precision requirement → evaluate once on the test set.**

F0.5 is useful when you want a single metric that weights precision more heavily, but a **precision constraint + recall optimization** often maps better to the actual requirements of an email spam filter.