---
sidebar_position: 8
---

# Decoding Text

After the model processes an Akkadian input, it must generate the corresponding English translation. This process of turning the model’s predictions into an output is called **decoding**. 

The model does not translate the Akkadian input all at once. Instead it predicts the output step by step. Each step, the model assigns distinct probabilities to possible outputs. 

**Example**:

> “This model…”

If the translation for an Akkadian sentence begins with the words quoted above, the model may consider several possibilities such as:

> “This model predicts…”
> “This model has…”
> “This model serves…”

Each possibility will be given a score on how likely it is correct. These scores will directly influence the final translation. (The model is based on ByT5 which generates text at the byte level rather than whole words. The examples are written in words since it is easier to visualize.)

## Beam Search

One common decoding method used in machine translation is **beam search**. 

Previously, we talked about the strategy of choosing the most likely next output after each step. This is often noted as **greedy decoding**. This method is not always the best, considering that selecting the most likely output does not always lead to the best complete translation. This can be avoided when using beam search, which keeps several promising translations at the same time instead of choosing just one. The number of possibilities kept is called the **beam width**. 

**Example:**

Let’s say that the beam width is 2. Using the same example above, the model scored them as: 

> “This model predicts…” → score: 0.44
> “This model has…” → score: 0.38
> “This model serves…” → score: 0.15

Instead of choosing the best option – in this case “This model predicts…” – beam search keeps the two highest scoring possibilities which would be:

> “This model predicts…”
> “This model has…”

The model will continue to generate the next possible translations from both: 

> “This model predicts the language…”
> “This model has become…”

They will each receive new scores, compared, and the strongest candidates are kept again. This process will repeat until the model reaches the end of the translation. Allowing several possibilities to be kept will produce better outcomes. 

## Definitions

1.  **Decoding**: The process of converting a model’s predictions into an output
    
2.  **Greedy decoding**: A decoding method that keeps the most likely output each step
    
3.  **Beam search**: A decoding method that keeps multiple likely outputs
    
4.  **Beam width**: The number of possible sequences that beam search keeps each step
    
5.  **Sequence score**: The value that represents how likely it would be as a possible output
    

## Sources
- https://huggingface.co/docs/transformers/generation_strategies
- https://d2l.ai/chapter_recurrent-modern/beam-search.html
