---
hide:
  - toc
---

# Part II: AI/ML-Specific Tooling

> *“You don’t just need tools—you need the right tools for AI.”*

---

Part II shifts the focus to the **core components of AI/ML development**—from GPU runtimes and tokenizers to transformer models and inference pipelines. These are the tools that directly impact how your models run, scale, and serve real-world tasks.

This section explains these tools *practically*—with examples drawn from real projects like sentiment analysis, image generation, and chatbot deployment. If you've ever wondered what a tokenizer really does or what "inference mode" actually means, you're in the right place.

✅ Chapter 7: What Is a GPU Runtime?  

- Learn why GPU runtimes matter, when you need them, and how to access them for free on platforms like Kaggle, Colab, or Hugging Face. We cover GPU vs CPU performance for training and inference, memory pitfalls, and runtime debugging.

✅ Chapter 8: Transformers, Tokenizers & Hugging Face Ecosystem  

- Dive into the modern backbone of AI—transformer-based models. We explain the role of tokenizers, model configs, and pipelines. You'll learn how to use `AutoTokenizer`, `AutoModel`, and `pipeline()` from Hugging Face to run tasks like sentiment classification or summarization.

✅ Chapter 9: Inference vs Training – Know the Difference  

- Too many tutorials blur the line between training and inference. This chapter breaks it down clearly: from model freezing and dropout behavior, to `model.eval()` vs `model.train()` modes. You’ll understand when you're truly “training” vs just running predictions.

✅ Chapter 10: Understanding Replicate & Stability API  

- Learn how to access powerful models (like Stable Diffusion, U-GAT-IT, etc.) via hosted inference APIs. This chapter covers endpoints, model versions, pricing patterns, authentication, and how to integrate these APIs into your own projects safely and efficiently.

✅ Chapter 11: Prompt Engineering Basics  

- Text-in, magic-out? Not quite. Learn how to craft prompts that consistently guide models toward the outputs you want. We explore few-shot prompting, role-conditioning, temperature settings, and real-world examples using OpenAI and Hugging Face models.

After Part II, You Will Be Able To:

* Choose the right compute runtime for your AI tasks
* Use transformer models and tokenizers with the Hugging Face ecosystem
* Distinguish between training mode and inference mode (and avoid costly mistakes)
* Integrate hosted models into your apps via Replicate or Stability AI
* Write better prompts that improve NLP task performance

---

> *This part gives you the core “language” of AI systems—the invisible configurations that make or break performance.*
