---
sidebar_position: 1
---

# NLP Overview

**Definitions:**
1.  Low resource language: generally lost, historical, or uncommonly written languages with a limited amount of available digital data, making standard data-heavy ML techniques difficult. 
2.  Pretraining: the initial stages of training on massive, unlabeled datasets. 
3.  Subwords: smaller textual units like roots, prefixes, or common letter combinations/sounds that can be used to identify rare or unknown words.
4.  Akkadian: an ancient extinct Semitic language written using a cuneiform script; classic example of a low-resource language challenge for linguistic computational research.
5.  Dependency parsing: certain words may modify or relate to others, so this task maps out these grammatical combinations, such as adjectives _depending_ on nouns (objects). 
6.  Lemmatization: reduced inflected word forms to their **lemma**. 
7.  Lemma: 
8.  Stemming: 
    

## What is Natural Language Processing (NLP)?

**Natural Language Processing** (NLP) is an aspect of artificial intelligence that allows computers to read, “understand”, and write human language without relying on specific keywords or a predetermined script. It is the primary tool that allows machines to bridge the gap between how people talk and how computers “think” or calculate. **Large Language Models** (LLMs) like Claude, ChatGPT, Gemini, or DeepSeak heavily rely on this technology. Virtual assistants including Apple’s Siri have also used NLP since 2011, and it is also what allows translation tools to function properly. NLP, notably, does not necessarily include the creation of novel content as generative AI does, and is rather a broader field of computer science focused primarily on understanding or interpreting language through analyzing existing data. 

## How are NLP models trained?

Within **pretraining**, models must initially discover words and **subwords** using data-driven algorithms in a process known as **tokenization.** 

Rather than being “given” a form of a traditional dictionary, algorithms build vocabulary purely on statistics and frequency. This ability to find meanings even for novel words or words with previously unknown meanings is what allows NLP to be effective even when processing ancient or lost languages, including **Akkadian**. 

## How does NLP work? 

The mandatory generic processes for a trained NLP model to function are as follows:

Tokenization occurs first, where the system breaks the inputted text down.

**Vectorization** occurs next. Since computers cannot read the words embedded in data directly, the tokens are converted into numerical vectors that correspond to meaning and/or context.

Grammar analysis analyzes the sentence structure in order to break down grammatical rules. This could include something like classifying words as equivalent to modern concepts of subject/verb/objects in context and where in each sentence each classification is commonly placed. It also performs **dependency parsing.** They must also reduce words to root forms via **stemming** or **lemmatization**. 

Semantic analysis follows, where the meaning for these root words are derived in context. This may involve techniques like _word sense disambiguation_ (in cases of words having multiple meanings, deriving which one is most accurate to the context). _Named Entity Recognition_ spots recurring names, dates, and locations, which assists in context identification.

In modeling and inference, transformer neural networks are used to learn patterns from data to classify text or provide translations. In other cases, it may be used to detect sentiment or answer questions directly.

Natural Language Generation (**NLG**) can then be used to convert calculated data back into human language and respond to a prompt.