---
sidebar_position: 6
---

# Encoding Text

Before our model can translate Akkadian into English, it must first convert text into numbers. This process is called **encoding**. Computers can’t directly understand words, so numerical representation helps them recognize patterns and relationships in language.

## 1\.  Word Embeddings

A **word embedding** represents a word as a list of numbers called a _vector_. Words that appear in similar contexts often have similar vectors. 

For example, ‘sister’ and ‘brother’ may have similar embeddings because they frequently appear in related situations.

One early method for learning these representations is **Word2Vec**.

## 2\.  Word2Vec

Word2Vec has two main training methods:
*   CBOW (Continuous Bag of Words)
*   Skip-gram

**CBOW** predicts a missing word using the words around it.

Example: “They planned a surprise \_\_\_\_ for their friend”

The model might predict ‘party’ as the word in the blank using the surrounding words.

**Skip-gram** does the opposite. It uses one word to predict nearby words.

Example: Given ‘party’ in ‘They planned a surprise party for their friend,’ the model learns to predict surrounding words such as ‘surprise’ and ‘for’

Both methods help computers learn relationships between words through repeated practice.

## 3\.  Akkadian Model

Our project uses ByT5 which works differently from Word2Vec.

Instead of dividing text into whole words, ByT5 processes UTF-8 bytes, small units of digital information. 

For example, the Akkadian word _šarrum_ (king) contains the special character ‘```š```’ which requires two UTF-8 bytes.

This is useful for Akkadian because its transliterations contain unfamiliar spellings, special characters, and variations that traditional word-based systems may struggle with.

ByT5 converts these bytes into learned numerical representations. Its Transformer architecture then processes their relationships to help generate an English translation.

## 4\.  Encoding in Python

We can demonstrate byte encoding with a few lines of code:

```
text = "šarrum"
encoded = list(text.encode("utf-8"))
print(encoded)
```

Output: 

\[197, 161, 97, 114, 114, 117, 109\]

<small>These are raw UTF-8 byte values, not the exact internal token IDs used by ByT5.</small>

<br> </br>
<br> </br>

**Summary**: Word2Vec demonstrates how computers can learn numerical representations of words. ByT5 builds on the broader idea of numerical representations but begins with bytes, allowing it to process Akkadian text without relying on a fixed vocabulary of complete words.

## Resources
- https://arxiv.org/abs/1301.3781
- https://aclanthology.org/2022.tacl-1.17/
- https://www.unicode.org/reports/tr17/
- https://oracc.museum.upenn.edu/epsd2/o0037236
- https://jalammar.github.io/illustrated-word2vec/?utm_source=chatgpt.com
- https://developers.google.com/machine-learning/crash-course/embeddings?utm_source=chatgpt.com