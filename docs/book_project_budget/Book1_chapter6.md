# 📘 Chapter 6: Integrating with Paid APIs

## 6.1 Why Use Paid APIs?

While open-source models are powerful, paid APIs:

- Require no setup or training.

- Are heavily optimized (fast inference).

- Provide access to state-of-the-art models like GPT-4, DALL·E, or CartoonGAN.

- Help you ship projects faster.

Examples of what you can do:

|Project Type	        |Paid API Option	        |Task Performed                     |
|-----------------------|---------------------------|-----------------------------------|
|Chatbot / Assistant	|OpenAI GPT-3.5/GPT-4	    |Generate responses                 |
|Meme Generator	        |OpenAI (captioning)	    |Funny/witty text                   |
|Cartoonizer	        |Replicate (CartoonGAN)	    |Image-to-cartoon transformation    |
|Image Generator	    |Stability AI (SDXL)	    |Generate art or visuals            |
|Translator	            |DeepL API / OpenAI GPT	    |Translate between languages        |

---

## 6.2 Using OpenAI API (Text-Based)

**Installation**
```bash
    pip install openai python-dotenv
```
**📂 .env (in backend/)**
```bash
    OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxx
```
**📄 backend/app/main.py**
```python
    import openai
    import os
    from dotenv import load_dotenv
    load_dotenv()
    openai.api_key = os.getenv("OPENAI_API_KEY")
    @app.post("/generate")
    def generate_caption(request: PromptInput):
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[
                {"role": "system", "content": "You are a funny meme caption generator."},
                {"role": "user", "content": request.prompt}
            ]
        )
        return {"caption": response['choices'][0]['message']['content']}
```
> You can switch to gpt-4 later by changing the model name.

---

## 6.3 Using Replicate API (Image-Based)
**installation**
```bash
    pip install replicate
```
**📂 .env**
```bash
    REPLICATE_API_TOKEN=r8_your_api_token_here
```
**📄 backend/app/cartoonize.py**
```python
    import replicate
    import os
    replicate.Client(api_token=os.getenv("REPLICATE_API_TOKEN"))
    def cartoonize_image(image_path):
        output = replicate.run(
            "tstramer/cartoonify:latest",
            input={"image": open(image_path, "rb")}
        )
        return output  # typically a URL
```

---

## 6.4 Securing API Keys in Production

✅ Best Practices:

- Use .env for local development.

- Use Secrets tab in Railway / Hugging Face Spaces / Vercel for production.

- In frontend projects, never expose API keys directly.

        ○ Frontend → calls backend → backend calls OpenAI.

---

## 6.5 Cost Control Tips (Prevent Exploding Bills)

|Tip	                        |Description                                                |
|-------------------------------|-----------------------------------------------------------|
|🔁 Limit request frequency	    |Add cooldown or delay between calls (e.g., 1 call/10s)     |
|📊 Monitor token usage	        |Log token usage per request (OpenAI provides this)         |
|🧠 Use smaller models	        |Prefer gpt-3.5-turbo instead of gpt-4                      |
|🚫 Block long prompts	        |Enforce input length limit from frontend                   |
|🔁 Add caching	                |Cache repeated results (e.g., for meme captions)           |
|💬 Summarize before sending	|If chaining user inputs, summarize old messages            |

> ✅ Hugging Face & Railway let you inspect logs and rate-limit usage if needed.

---

## 6.6 Testing with Rate Limits
Here’s a simple example using a manual throttle:
```python
    import time
    last_request_time = 0
    def call_api_throttled(prompt):
        global last_request_time
        now = time.time()
        if now - last_request_time < 5:  # 5 sec cooldown
            return {"error": "Please wait before trying again."}
        last_request_time = now
        # continue with API call
```

For production, you can use: 

- Redis for persistent rate tracking  

- FastAPI middleware to track usage per IP/user

---

## 6.7 When to Use Paid APIs (vs Free Models)

|Situation	                    |Use Paid API? ✅               |
|-------------------------------|-------------------------------|
|You need quick prototyping	    |✅ Yes (fastest to deploy)     |
|You’re demoing for recruiters	|✅ Yes (polished output)       |
|You’re building MVP for users	|✅ Yes (lower risk)            |
|You’re training custom models	|❌ Use open-source             |
|You want offline access	    |❌ Use local inference         |
|You’re processing huge volume	|❌ May be too expensive        |

> Start with APIs, then optimize with free or local alternatives when scaling.

--

## ✅ Chapter Summary

- You’ve integrated OpenAI and Replicate APIs into your backend.

- Your keys are secured with .env and deployment secrets.

- You’ve added cost control, safe request handling, and fallback logic.

- You're now ready to build production-level AI features efficiently!

---
