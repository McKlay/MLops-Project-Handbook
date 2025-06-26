---
hide:
  - toc
---

# Chapter 11: Prompt Engineering Basics

*“Talk to models like you mean it.”*

This chapter is one of the most **human** in this journey — it’s not about code, or containers, or GPUs. It’s about **language**.
And how we, as builders, can shape AI behavior simply by choosing the right words.

Welcome to the artful world of **Prompt Engineering**.

---

## This Chapter Covers

* What prompt engineering is (and why it matters)
* The structure of a good prompt
* Task types: classify, generate, reason, summarize
* Techniques: instruction, examples, tone, constraints
* Builder’s lens: shaping intelligence through intention

---

## Opening Reflection: Words as Levers

> *“Give me the right words, and I will move the model.”*

You don’t need to change the architecture.
You don’t need to retrain the weights.
You don’t even need to touch the API.

You just need to say the right thing — in the right way — and watch the model become:

* A poet
* A helper
* A data analyst
* A sarcastic comedian
* A highly specific meme captionist

This is **prompt engineering**:
The art of talking to machines… and getting exactly what you meant.

---

## 11.1 What Is Prompt Engineering?

**Prompt engineering** is the craft of designing inputs to language models (e.g. GPT, Claude, Mistral) that guide the model to produce useful, accurate, or creative output.
It’s part programming, part psychology, part UX.

---

## 11.2 Prompt Structure: The Core Recipe

```
Instruction  
(Optional) Examples  
Constraints / Formatting  
User Input
```

### Example (Sentiment Analysis Prompt):

> Classify the sentiment of the following sentence as Positive, Negative, or Neutral:
> Input: "I don’t love this product, but it works."
> Sentiment:

The model fills in the blank: `"Neutral"`

---

## 11.3 Common Prompting Tasks

| Task Type      | Goal                      | Example Prompt                                 |
| -------------- | ------------------------- | ---------------------------------------------- |
| Classification | Label text (e.g., intent) | “Label this text as Happy, Sad, or Angry.”     |
| Summarization  | Compress info             | “Summarize this article in 3 bullets.”         |
| Generation     | Produce text              | “Write a tweet about AI in 10 words.”          |
| Reasoning      | Chain of logic            | “Explain why gravity decreases with distance.” |
| Extraction     | Pull structure from chaos | “Extract dates and names from this text.”      |

---

## 11.4 Techniques That Boost Prompt Quality

| Technique         | Example / Effect                    |
| ----------------- | ----------------------------------- |
| Explicit roles    | “You are a meme caption generator…” |
| Few-shot learning | Show 1–3 examples before user input |
| Chain of Thought  | “Let’s think step by step…”         |
| Output formatting | “Respond in JSON: {‘label’: … }”    |
| Style injection   | “Respond as if you're Shakespeare.” |

---

## 11.5 Prompt Engineering in Code (OpenAI)

```python
import openai

response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "You are a product description writer."},
    {"role": "user", "content": "Describe a smart water bottle for athletes."}
  ]
)

print(response["choices"][0]["message"]["content"])
```

This is where the **magic** happens.

---

## 11.6 When Prompting Fails: Debug Like a Builder

| Symptom               | Try This Fix                              |
| --------------------- | ----------------------------------------- |
| Too generic / vague   | Add examples or clarify instruction       |
| Output too long/short | Add constraints: “<30 words” or “3 lines” |
| Repeats itself        | Add: “Do not repeat yourself.”            |
| Hallucinates info     | Add: “Only use the info provided.”        |

Prompting is **iterative**.
You’ll get better through **play**.

---

## 11.7 Builder’s Perspective: Your Prompt Is a Prototype

> *“In a world where models know everything,
> what matters is how you ask the question.”*

Your prompt:

* Is your interface
* Is your architecture
* Is your business logic
* Is your UX

It's the single string of text that determines whether your app feels:

* Confident
* Helpful
* Funny
* Human

You don’t need to **know more**.
You need to **ask better**.

---

## Summary Takeaways

| Concept                      | Why It Matters                           |
| ---------------------------- | ---------------------------------------- |
| Prompting = shaping behavior | No code changes needed                   |
| Clear, specific input        | More reliable, useful outputs            |
| Few-shot + structure help    | Reduces hallucination, increases control |
| Prompt = soft interface      | Easy to change, test, and improve        |

---

## 🌟 Closing Reflection

> *“The model has intelligence.
> You have intention.
> Prompt engineering is the conversation between the two.”*

---