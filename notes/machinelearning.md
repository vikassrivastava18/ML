### What is Machine Learning?
```
Machine Learning is a technique where a model learns patterns or relationships from historical data instead of relying on explicitly programmed rules. The model adjusts it's paramters training so that it can make accurate predictions on new unseen data
```

### Is the model memorizing or generalizing
```
The model learns general patterns and relationships from the training data. If instead it memorizes individual training examples, it may fail to generalize to new, unseen data. This is called overfitting.
```

### What does this indicate?
| Dataset  | Accuracy  |
| -------- | --------- |
| Training | **99.8%** |
| Testing  | **61%**   |

```
This indicates overfitting which is caused by high variance.
```

### How to avoid overfitting

```
Onec can avoid overfitting through many techniques
    - Adding L1, L2, regularization
    - Adding droputs to the hidden layers
    - Collect more data (if feasible)
    - Reduce model complexity
    - Early stopping
    - Feature selection (remove noisy or irrelevant features)
    - Cross validation for hyperparameter tuning
```

### Which model to pick

| Model | Train Accuracy | Test Accuracy |
| ----- | -------------- | ------------- |
| A     | 99%            | 60%           |
| B     | 82%            | 80%           |

```
I would choose Model B because the goal of machine learning is to perform well on unseen data (indicated by test accuracy), so B is better.
```

### 
If the test set is used to evaluate the model, why don't we use the test accuracy to tune hyperparameters?

```
That will cause data leakage because we will use test data to tune the model, therefore model does is biased by the test data.
```

### Can linear regression model non-linear relationships?

```
Yes. Linear regression can model nonlinear relationships by transforming the input features, such as adding x**2, x**3. This is known as polynomial regression.
```

### Precision
"When my model says spam, how often is it a spam?"
```Out of all the samples predicted as positive, hom many were actually positive?```

### Recall
"Of all the cancer patients, how many did we find?"
```Out of all the positive cases, how many did the model precit successfully```

F1 - Checks for both and balances both precision and recall. Useful for imbalanced datsets








