```
├── Module-1.ipynb    # OpenAI and Gemini AI Setup and Text Generation
├── Module-2.ipynb    # Sentiment Analysis and NER with Hugging Face
├── Module-3.ipynb    # Question Answering with BERT
├── Module-4.ipynb    # Text Classification with XLNet=
├── config.py         # API Key Configuration
```

### What are LLMs?

* LLM = **Large Language Model**.
* They are AI models trained on huge amounts of text.
* Main job: **predict the next word** in a sentence.
* Because of this, they can **understand, generate, and reason with language**.

---

### How Large is an LLM?

* Size = number of **parameters** (like memory slots).
* Small models → millions of parameters.
* Big models → billions (even trillions) of parameters.
* Larger models usually = **smarter**, but need more power to train and run.

---

### General Purpose Models

* Trained on **all kinds of data** → not limited to one task.
* Can answer questions, write stories, summarize, code, translate, etc.
* Act as **foundation models** → can be adapted for special use.

---

### Pre-training and Fine-tuning

* **Pre-training:** Model learns from a **giant dataset** (general world knowledge).
* **Fine-tuning:** Model is given **extra training** on specific tasks (e.g., medical Q\&A, customer support).
* This combo makes LLMs both **broad** and **specialized**.

---

# Transformer Architecture

### 🔹 What is a Transformer?

A **Transformer** is a type of deep learning model architecture introduced in the paper **“Attention is All You Need” (2017)**.

* It is the backbone of modern **Large Language Models (LLMs)** like GPT, BERT, LLaMA, etc.
* Transformers are designed to handle **sequences** (like text), but unlike older models (RNNs, LSTMs), they can look at **all words at once** using **attention**.

---

### 🔹 Transformer Architecture

<img width="1264" height="1075" alt="Screenshot 2025-09-15 153440" src="https://github.com/user-attachments/assets/571b42db-989c-46e1-9ee6-21c595faa3bb" />


## 🔹 Encoder (Reader) – Step by Step

The **encoder**’s job is to **understand the input text** and turn it into meaningful information the decoder (writer) can use.

---

### 1. **Input Embedding**

* Words (like "I", "love", "cats") are turned into **vectors** (numbers) using embeddings.
* Each word gets a unique vector that captures its meaning.
* Example: "cat" and "dog" vectors will be closer than "cat" and "table".

---

### 2. **Positional Encoding**

* Transformers don’t naturally understand **word order** (who comes first, second, etc.).
* Positional encoding adds a small pattern of numbers that tells the model:

  * "I" is word 1, "love" is word 2, "cats" is word 3.
* This helps the model know that “cats love I” is different from “I love cats”.

---

### 3. **Multi-Head Self-Attention**

* This is the **heart** of the transformer.
* Each word looks at **other words in the sentence** to understand the context.
* Example:

  * For “The cat sat on the mat because it was tired” →

    * The word **“it”** pays more attention to **“cat”** than “mat”.
* **Multi-head** = it looks at the sentence from multiple angles (different “heads” focus on different relationships, like subject–verb, adjective–noun).

---

### 4. **Add & Norm (Residual + Normalization)**

* After attention, the output is **added back** to the original input (residual connection).
* Then it’s **normalized** (keeps numbers stable and avoids exploding/vanishing values).
* This makes training faster and more stable.

---

### 5. **Feed Forward Network**

* A simple neural network that processes the information further.
* Think of it as a small filter that polishes what attention found.

---

### 6. **Add & Norm (again)**

* Another residual connection + normalization step, same as before.

---

### 7. **Stacking (Nx layers)**

* The above steps (Attention → Add & Norm → Feed Forward → Add & Norm) are repeated **N times** (like 6, 12, 24 layers).
* Each layer refines the representation more deeply.

---

### ✅ Output of Encoder

* After all layers, the encoder produces a **set of vectors** that represent the meaning of the input sentence with context.
* Example:

  * "cats" isn’t just “animal”, but in context → "subject of sentence", "related to tired".

---

👉 In short:
The **encoder** takes words → turns them into vectors → adds order info → lets words look at each other (attention) → refines → outputs a smart representation of the whole input.

---

## 🔹 Decoder (Writer) – Step by Step

The decoder produces the final text. It uses:

1. What has already been generated (previous words).
2. What the encoder understood about the input.

---

### 1. **Output Embedding**

* Just like the encoder, words in the output (already generated) are turned into **vectors**.
* Example: If the model has already generated "I love", those two words are turned into vectors.

---

### 2. **Positional Encoding**

* Adds order information so the model knows the position of each word in the output sentence.
* Example: "I" = position 1, "love" = position 2.

---

### 3. **Masked Multi-Head Self-Attention**

* This is the **first attention block in the decoder**.
* **Masked** means: when predicting the next word, the model is **not allowed to look at future words**.

  * Example: When generating "cats", it shouldn’t peek at "are cute" that comes later.
* Like the encoder’s attention, but restricted to avoid cheating.

---

### 4. **Add & Norm**

* Residual connection (adds back original input) + normalization (stabilizes numbers).

---

### 5. **Multi-Head Attention (with Encoder Output)**

* This is where the decoder connects with the encoder.
* It takes the **context from the encoder** (what the input sentence means) and combines it with the partially generated output.
* Example: If encoder processed “Me gusta los gatos” (Spanish), the decoder uses that info to generate “I like cats” in English.

---

### 6. **Add & Norm**

* Again, residual + normalization to stabilize learning.

---

### 7. **Feed Forward Network**

* A small neural network that polishes the combined info.

---

### 8. **Add & Norm**

* Same step again (residual + normalization).

---

### 9. **Stacking (Nx layers)**

* Just like the encoder, these steps are repeated multiple times (6, 12, 24 …).

---

### 10. **Linear + Softmax Layer**

* **Linear**: Converts the decoder’s output into raw scores (logits) for each possible word.
* **Softmax**: Turns those scores into probabilities.
* The word with the highest probability is chosen as the next word.
* Example: For “I love”, the model might predict:

  * cats (0.82), pizza (0.12), running (0.05).
  * It picks **“cats”**.

---

### ✅ Output of Decoder

* Generates the final sequence **one word at a time**, using previous outputs + encoder context.
* Example:

  * Input: “Me gusta los gatos”
  * Output: “I like cats” (produced word by word).

---

👉 In short:
The **Decoder** looks at:

* Its own past words (masked self-attention).
* The encoder’s understanding of the input (cross-attention).
* Then step by step, it predicts the next word until the sentence is complete.

---
