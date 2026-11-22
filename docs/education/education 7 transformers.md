---
sidebar_position: 7
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import Link from '@docusaurus/Link';

# Transformers

## 1\.  What is a transformer?
    
A transformer is a **neural network** architecture built for working with sequences, such as sentences. Before transformers, most translation models read each word of a sentence one at a time. This can pose issues during **encoding**, since the meaning of a word can change depending on its position in a sentence, and different languages have different sentence structures; the **encoder** would struggle to represent the meaning of the word accurately. 

In Akkadian, the verb usually comes at the end of the sentence, while in English, it comes in the middle:

<p align="center">šarrum ālam ikšud = “The king conquered the city.”</p>

where ikšud means “he conquered.” The model would need to know and recognize the subject (šarrum) and  the direct object (ālam) in order to translate the sentence.

To solve this issue, transformers abandon that approach and use **attention** instead. Most models today use attention-based transformer architectures.

Definition:
1. **Attention**: allows the model to look at the sequence/sentence as a whole rather than just one word at a time

We will provide a _very_ high-level overview of what transformers are (you can read more [here](https://proceedings.neurips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html)). To understand how a transformer works, we reference the diagram below. 

<img src={useBaseUrl('img/transformer_diagram.png')} style={{ width: 600 }} alt="Transformer Diagram"/>

A transformer consists of an encoder (left) and a decoder (right). (You can read about encoders and decoders [here](education%206%20encoding) and [here](education%208%20decoding).) The encoder and decoder are made of sub-layers (Multi-Head Attention, Feed Forward) that form a **layer** (gray box), and the “```Nx```” on the sides means “repeat N times.”

1.  Embedding and Positional Encoding
    
The input is embedded as a vector. In our case (ByT5), this means each byte is represented using numbers. Because a transformer looks at all pieces of a sentence at once, it needs a **“positional encoding”** to keep track of the ordering of the pieces. The paper uses trigonometric functions (```cos```, ```sin```) to do this.

## 2\.  Multi-Head Attention
    

Each byte has a **query**, a **key**, and a **value**. A **query** represents what the byte is looking for, its **key** represents what the byte contains and is compared against incoming queries from other bytes, and its **value** represents the meaning of the byte, or the information it holds.

The model then compares each query to every key, and assigns them a score (**dot product** of the query and key). A good match gives a high score. During training, the mode learns how to match queries and keys together to produce higher scores. 

“Multi-head” means that the model does this several times at once (in parallel), and each “head” can look for something different. For example, one head might be linking a pronoun to the noun it refers to, while another head might be linking a subject to its corresponding verb.

## 3\.  Add & Norm

**Add** sums the input (a vector) of a sub-layer (e.g. Multi Head Attention, Feed Forward) to the output of the sub-layer (a new vector). The purpose of this is to ensure that the model retains information from the original input. Equivalently, if ```x``` is the input, this can be written as

```x + Sublayer(x)```

**Norm** normalizes the sums from **add** so the entries of the vectors stay within a certain range, and the output is

```LayerNorm(x + Sublayer(x))```.

## 4\.  Feed Forward 

Each byte’s vector goes through a small two-layer neural network that updates it so that it is ready for the next layer.

## Linear and Softmax

At the end, the decoder’s output vector is turned into a probability for every next possible byte, and the most likely byte is chosen:

**Linear** is a function that turns the decoder’s output vector into a vector of scores for each possible byte.

**Softmax** turns the scores into a probability distribution (entries of the vector sum to 1 and are in ```[0, 1]```). The model then picks the byte with the highest probability using **beam search**. 

## Sources

* https://proceedings.neurips.cc/paper_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf