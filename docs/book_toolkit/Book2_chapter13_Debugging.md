---
hide:
  - toc
---

# Chapter 13: Logging, Monitoring & Debugging for AI Apps

*“What you can’t see… you can’t fix.”*

This chapter is where **intuition meets visibility**. Because after you build and deploy an AI app, the most important question becomes:
**“What’s actually happening under the hood?”**
This chapter gives you the tools and mindset for **logging, monitoring, and debugging** your AI systems — so you can fix bugs, track usage, and scale with confidence.

---

## This chapter covers:

* What logging and monitoring mean for AI/ML projects
* The difference between print, logs, metrics, traces
* Logs in FastAPI, React, and deployment platforms
* Debugging techniques for model pipelines
* Builder’s lens: visibility as power

---

## Opening Reflection: Seeing the Invisible

> *“The best AI apps don’t just think.
> They reflect — and report.”*

Imagine a user types in a prompt… but nothing happens.
Or your backend runs fine locally… but fails silently in production.
Or worse — your model is returning bad predictions, and no one notices.

**Logs are your eyes inside the machine.**
**Monitoring tools are your heartbeat monitor.**
**Debugging is your path to insight.**

You don’t need to guess what’s wrong.
You need to ask the system to speak.

---

## 13.1 Logging vs Monitoring vs Debugging

| Term       | What It Means                             | Example                      |
| ---------- | ----------------------------------------- | ---------------------------- |
| Logging    | Human-readable traces of what happened    | `print("Received input...")` |
| Monitoring | Graphs & metrics over time (infra, usage) | RAM usage, API call count    |
| Debugging  | Interactive tracing of bugs/errors        | Try/except, breakpoints      |

---

## 13.2 Logging in FastAPI

```python
import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

@app.post("/predict")
async def predict(input: InputModel):
    logger.info(f"Received input: {input}")
    result = my_model.predict(input.text)
    logger.info(f"Prediction: {result}")
    return {"result": result}
```

✅ Logs show up in:

* Local dev terminal
* Railway/Render logs
* Hugging Face Spaces log console

---

## 13.3 Logging in React (Frontend)

```js
console.log("User clicked submit.");
console.warn("No input detected.");
console.error("Failed to fetch from backend.");
```

In production, you can route logs to:

* Vercel Analytics (PRO)
* LogRocket
* Sentry

You can track:

* Button clicks
* Page views
* Inference time per generation

---

## 13.4 Model Debugging: Watch the Flow

If your AI app isn’t working as expected:

| Symptom                   | Possible Cause                    | Fix                           |
| ------------------------- | --------------------------------- | ----------------------------- |
| Empty / wrong predictions | Model not loaded, bad input shape | Log input/output, check types |
| Timeout or 502 error      | Large model loading, cold start   | Add loading spinner, preload  |
| App crashes after upload  | Wrong file format or MIME type    | Use `PIL.Image.verify()`      |

Always log:

* What was received
* What was processed
* What was returned

---

## 13.5 Monitoring Tools (When to Scale)

| Tool             | What It Tracks              | Great For                   |
| ---------------- | --------------------------- | --------------------------- |
| Prometheus       | Custom metrics (in FastAPI) | DIY infra / backend metrics |
| Sentry           | Frontend + backend errors   | Crash reports + debugging   |
| Railway Logs     | App logs and performance    | Most AI projects in dev     |
| Vercel Analytics | Page views, traffic, usage  | React dashboards            |

---

## 13.6 Add Your Own Analytics

**utils/logger.py**

```python
import datetime

def log_usage(endpoint: str, user_input: dict):
    with open("logs.txt", "a") as f:
        f.write(f"{datetime.datetime.now()} | {endpoint} | {user_input}\n")
```

✅ Append logs to local file or S3
✅ Helps track usage patterns
✅ Later used for analytics, dashboards, limits

---

## 13.7 Builder’s Lens: Debugging = Listening to Your App

> *“Your app is speaking. The logs are its voice.”*

As AI builders, we don’t just write code.
We write conversations with the unknown.

Logs tell us:

* When users are confused
* When something fails quietly
* What needs to scale or evolve

**When you listen to your app, you hear its soul.**

---

## Summary Takeaways

| Insight                   | Why It Matters                       |
| ------------------------- | ------------------------------------ |
| Logging = awareness       | Know what happened, when, and how    |
| Monitoring = metrics      | See trends over time (cost, traffic) |
| Debugging = investigation | Trace cause → fix with precision     |
| Use logs early            | Don’t wait for bugs to get serious   |

---

## 🌟 Closing Reflection

> *“AI is not magic.
> It’s engineering — and good engineers listen.”*

---