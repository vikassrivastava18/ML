## Why do we need some non-linearity through activation functions ReLU or Sigmoid?

```
We need some non-linearity through activation functions like ReLU otherwise the hidden layers do not serve much purpose and can be replaced by a single layer
```

## Why ReLU, not Sigmoid nowadays?
```
Sigmoid is rarely used in hidden layers because it saturates for large positive or negative inputs, where it's derivative becomes close to zero. During backpropogation, gradients are multiplied accross layers, so these these small derivatives cause the gradients to vanish before reaching earlier layers. As a result those layers learn very slowly. ReLU alleviates this problem because it's derivative is 1 for positive inputs, allowing gradients to propogate more effectively. However, ReLU can suffer from the dying ReLU problem, which variants like leaky ReLU address.
```

## What is backpropogation
```
During the forward pass, the network computes activations layer by layer until it produces a prediction and evaluates a loss. During backpropogation, we compute the gradient of the loss with respect to the output layer and then use the chain rule to efficiently propogate these gradients backward through the NN. This give the graient of the loss with respect to the parameters. We propogate the gradients backward because the error is only known after prediction has been compared with the target
```

## Why backpropogation
```
Backpropogation is an efficient application of the chain rule. Instead of computing the gradient od each weight independantly, it computes gradients layer by layer while reusing intermediate derivatives. This avoids reduntant calculations and makes it practical to train neural networks for millions/billions parameters.
```

## Symmetry problem
```
The problem arises when neurons have identical parameters and therefore produce identical outputs and gradients. Randomly initializing the weight is sufficient to break the symmetry because neurons repond differently to the same input. Although the biases can all start at zero, they recieve different gradients due to different activations produced by random weights, so they quickly divege during training.
```

## Batch normalization
```
Before passing the activations to the next layer, we noralize them so that they have approximately zero mean and unit variance. As a result the next layer sees a much more stable input distribution from batch to batch.
It stabilizes activations and makes optimization easier.
```

## Dropout
```
Dropout is a regularization technique in which a random subset of neurons is temporarily disabled during training. This prevents neurons from co-adapting or relying heavily on specific other neurons. As a result, each neuron learns more robust and independent features, reducing overfitting and improving generalization. During inference, dropout is disabled and the full network is used.
```
