---
sidebar_position: 5
---

# Training a Neural Network

For a machine to learn how to translate a language, **training** is needed. Training involves showing a model examples repeatedly, measuring how well it predicts the correct output, and adjusting the internal parameters so that its predictions improve. 

Our project uses **ByT5**, a pretrained neural network that works with UTF-8 bytes rather than a vocabulary of whole words or subwords. Instead of building a model from scratch, we **fine-tune** an existing ByT5 model on paired Akkadian texts and English translations. This allows the model to adapt what it learned during pretraining to a specific translation task. 

In this section, we will explain the four central ideas behind training: **loss functions, cost functions, gradient descent, and backpropagation**.

## 1\. What does the model learn from?

To train a translation model, we need **parallel corpus**. These are examples containing a source-language text and its corresponding target-language translation. In our case, the source is Akkadian, which is represented in Latin letter transliteration, and the target-language is English. 

The model’s job is to assign higher probabilities to the bytes that make up the correct English translation. ByT5 uses bytes instead of vocabulary words so it mainly converts text to byte IDs rather than learning a word or subword. 

## 2\. Making a prediction

First, the model processes the Akkadian input and begins predicting an English output. This is called the **forward pass**. 

Broadly, this is what it looks like:

Akkadian text → ByT5 model → probabilities for the next English output byte

During training, the model has access to the correct English target. In a method called **teacher forcing**, the decoder is given the correct earlier output units while learning to predict the next one. During actual translation, those correct English units are not given, so the model must generate its output step by step. 

Then, how do we know whether a prediction is good? We calculate its loss. 

## 3\. Loss functions

A **loss function** measures how different the model’s prediction is from a desired output. Lower loss generally means the model assigned a greater probability to the correct answer. 

For our translation model ByT5, training typically uses cross-entropy loss. At each output position, the model assigns a probability to each next potential byte of its English output.. 

The loss for a single position would look like:

```-ln(probability assigned to the correct byte)```, where ```ln``` means the natural log.

Intuitively, this makes sense, since assigning a higher probability to the correct byte results in a lower loss. For example, if the probability is 1, then the loss is ```-ln(1) = 0```, but if the probability is only 0.1, the loss is ```-ln(0.1) ≈ 2.3```.

A real translation contains many output bytes. Its sequence loss combines the losses across the target positions, typically by averaging them while excluding artificial padding positions. 

A simplified sequence loss function can be written like this:

```L = -(1/T) * Σ_t log(P)``` (correct byte at position t | Akkadian input, preceding correct bytes)

T is the number of output positions included in the calculation. P is the probability assigned by the model. This equation is more complex than the one-byte translations but the principle is consistent. You can learn more about it [here](https://huggingface.co/docs/transformers/tasks/translation).

## 4\. Cost functions

One Akkadian-English pair translation does not tell us how well the model translates overall. We need to consider many examples in order to get the most accurate results.

It is useful to distinguish the two terms like this:

*   **Loss**: The error for one training example (or one output position)    
*   **Cost**: The loss across multiple examples, such as a batch
    
For a batch containing B examples:

- ```Cost = (Loss_1 + Loss_2 + … + Loss_B) / B```

In machine learning resources, loss and cost are sometimes used interchangeably. The important idea is that the training process needs a numerical measure of its performance from the examples it is learning from.

## 5\. Gradient descent

Now that we know how to measure mistakes, we will learn how the model can improve. 

Neural networks contain many adjustable numbers called **parameters**, including weights and biases. These parameters influence the model’s prediction outcomes. Training changes them to reduce the cost. For those familiar with calculus, the goal is similar to finding a local minimum of a function, except we are now dealing with multiple variables. 

**Gradient descent** is the basic optimization idea behind this process. The gradient tells the model which direction would increase the cost most quickly, so moving in the opposite direction reduces it. We repeat this step many times, reducing the cost until it only improves negligibly. 

The basic update rule is:
*   new parameter = old parameter - learning rate x gradient

Mathematically, this can be expressed as:
*   ```θ(new) = θ(old) − η∇C(θ(old))```
    *   ```θ``` is a vector containing the model’s adjustable parameters
    *   ```η``` is the learning rate, which controls the size of an adjustment
    *   ```∇C``` describes how the cost changes when the parameters change (gradient)
        
**Example:**

For simplicity, consider the ith entry of ```θ```, which we will denote as ```θi```. This corresponds to the ```i```th weight. 

Suppose ```θ```(old)```i``` = 2.00, the ith component of the gradient, ```∇Ci```, is +0.40, and the learning rate ```η``` is 0.10

**Then, ```θ```(new)```i```** **\= 2.00 - (0.10 x 0.40) = 1.96**

The algorithm changes this weight from 2.00 to 1.96. 

A very small learning rate may make training slow. A very large one can cause unstable adjustments. It is therefore important to choose an appropriate rate. The learning rate we used was ```1e-4```, or 0.0001.

## 6\. Backpropagation

Gradient descent tells us how to use gradients, but we still need a way to find them.

That is the purpose of **backpropagation**. After the model makes predictions and calculates the loss, backpropagation works backward through the neural network to determine how each parameter contributed to that loss. It uses the chain rule (from calculus) to connect changes across layers. 

The process looks like this:
1.  Forward pass: Model processes Akkadian text and predicts English output bytes
2.  Calculate loss: compare the predicted probabilities with the correct English translation
3.  Backpropagation: Calculate gradients showing how the loss depends on model patterns.
4.  Optimizer step: Use those gradients to update the parameters.
5.  Repeat: Learn from more examples and check performance on data not used for updates.

It is important to note that backpropagation calculates gradients, while the optimizer uses them to update parameters. They work together, but are not the same thing.

## 7\. Batches, Epochs, Fine-tuning

A large training dataset is usually divided into smaller groups called mini-**batches**. Training on a batch produces a loss and an update to the model. One complete pass through the training dataset is called an **epoch**.

These are **hyperparameters**: choices made by the people training the model, rather than values the model learns automatically.

You might ask, why fine-tune ByT5 instead of starting over?

Pretraining gives a model broad knowledge of patterns in text through a large, general dataset. Fine-tuning continues training from those existing parameters using examples relevant to one specific task.

ByT5 was pretrained on multilingual text before our project began. Our team then used Akkadian-English pairs to adapt it for translation. This is especially valuable for a low-resource language such as Akkadian, where paired, digitized examples are limited and pretraining a large model from scratch would require substantial resources.

## 8\. How we know whether the model is improving

A common way to check progress is to separate examples into:
*   Training data: used to adjust model parameters
*   Validation data: used to compare versions of the model and guide development
*   Test data: held aside for a final evaluation of the selected model

We then record the loss on both the training and validation data.

## Sources
- https://aclanthology.org/2022.tacl-1.17/
- https://huggingface.co/docs/transformers/model_doc/byt5
- https://huggingface.co/docs/transformers/tasks/translation
- https://huggingface.co/docs/transformers/v4.48.2/en/model_doc/t5
- https://developers.google.com/machine-learning/crash-course/linear-regression/loss
- https://developers.google.com/machine-learning/crash-course/linear-regression/gradient-descent
- https://developers.google.com/machine-learning/crash-course/linear-regression/hyperparameters
- https://docs.pytorch.org/tutorials/beginner/basics/autogradqs_tutorial.html
- https://developers.google.com/machine-learning/crash-course/overfitting/dividing-datasets
- https://www.youtube.com/watch?v=aircAruvnKk
- https://www.youtube.com/watch?v=IHZwWFHWa-w