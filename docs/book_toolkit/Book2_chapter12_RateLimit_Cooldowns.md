---
hide:
  - toc
---

# Chapter 12: Rate Limits, Cooldowns & Billing Safety

**“Control is the first feature of scale.”**

This chapter focuses on something every developer needs to master early: cost control, rate limits, and user safety mechanisms.
Because it’s not just about making a smart app — it’s about making one that doesn’t surprise you with a \$500 bill.

---

## This Chapter Covers

* Why AI apps need usage control
* Rate limiting vs cooldowns vs quotas
* How to prevent API abuse (especially with OpenAI/Replicate)
* Billing guardrails and alert setups
* Builder’s lens: safety as a service

---

## Opening Reflection: The Cost of Every Click

> “A single API call costs cents. A few thousand? That’s your rent.”

You’ve done it — your app is live.
People are clicking “Generate,”
sending prompts, uploading selfies, hitting `/predict`.

But behind the scenes:

* OpenAI is charging per 1K tokens
* Replicate is charging per image processed
* Your free tier is disappearing like steam

Suddenly, your fun AI meme generator…
is costing real money — and fast.

Welcome to the part of AI dev no one talks about: cost safety.

---

## 12.1 Why This Matters

Every time a user:

* Sends a prompt to GPT
* Uploads an image to Replicate
* Requests a Hugging Face inference

You’re paying for it — or burning compute hours.

Without limits, your app is:

* Vulnerable to spam
* Expensive at scale
* Unpredictable in usage patterns

---

## 12.2 The 3 Layers of Cost Control

| Layer      | What It Means                | Example Tool / Method         |
| ---------- | ---------------------------- | ----------------------------- |
| Rate Limit | Max calls per minute/hour    | “5 requests per minute”       |
| Cooldown   | Delay between calls          | “Wait 10 seconds after click” |
| Quota      | Max total calls per user/day | “100 calls per user/day”      |

These can be implemented at:

* Backend level (e.g. FastAPI)
* Frontend level (e.g. React/Gradio logic)
* API provider level (e.g. OpenAI usage limits)

---

## 12.3 How to Rate Limit in FastAPI

**Install:**

```
pip install slowapi
```

**main.py:**

```python
from fastapi import FastAPI, Request
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)
app = FastAPI()
app.state.limiter = limiter

@app.get("/predict")
@limiter.limit("5/minute")
async def predict(request: Request):
    return {"result": "OK"}
```

This stops users from overloading your endpoints.

---

## 12.4 Cooldown (Frontend Style)

**React snippet:**

```javascript
const [lastUsed, setLastUsed] = useState(null);
const cooldown = 10000; // 10 seconds

function handleClick() {
  const now = Date.now();
  if (lastUsed && now - lastUsed < cooldown) {
    alert("Please wait a moment before trying again.");
    return;
  }
  setLastUsed(now);
  // Call backend
}
```

Prevents users from spamming “Generate” or “Submit” buttons.

---

## 12.5 Quotas Per User

Store user usage in:

* Firebase
* Supabase
* Tiny JSON file or SQLite

**Example logic:**

```python
if user_usage_today >= MAX_DAILY_QUOTA:
    return {"error": "You’ve hit today’s limit. Try again tomorrow."}
```

Great for freemium models or early monetization.

---

## 12.6 Billing Safety with APIs

**OpenAI**

* Set usage caps per API key at [platform.openai.com/account/usage](https://platform.openai.com/account/usage)
* View per-request logs and token counts
* Get billing alerts via email

**Replicate**

* See run costs before calling each model
* Monitor credit balance in dashboard
* Rotate tokens every 30 days

**Hugging Face**

* No billing unless using Inference Endpoints
* Free-tier RAM/CPU limits will throttle requests

---

## 12.7 Builder’s Lens: Guardrails Are a Service

> “A good AI app doesn’t just respond fast.
> It responds responsibly.”

Rate limits aren’t just about saving money.
They’re about:

* Building trust with users
* Preventing accidental overuse
* Supporting sustainable scaling

In fact, adding usage rules early on tells your users:
**“This tool is stable. You can rely on it.”**

---

## Summary Takeaways

| Safety Layer   | Why It’s Important                          |
| -------------- | ------------------------------------------- |
| Rate limits    | Prevents request spam                       |
| Cooldowns      | Controls behavior from frontend             |
| Quotas         | Helps enforce freemium tiers or budget caps |
| Billing alerts | Protects you from financial surprises       |

---

## 🌟 Closing Reflection

> “Creativity needs power.
> But power without control is chaos.”

---