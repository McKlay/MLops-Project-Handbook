---
hide:
  - toc
---

# Chapter 10: How to Stay Within Free Tiers

With great deployment comes great responsibility, Chapter 10 is all about smart cost control and usage monitoring — the key to sustainably running your AI apps, especially when using paid APIs (like OpenAI/Replicate) or free-tier compute (Railway/Hugging Face/Vercel). Let’s protect your wallet while scaling your impact.

---

## 10.1 Why Cost Optimization Matters

Even small AI/ML apps can rack up unexpected costs due to:  
- Token overuse (OpenAI, Claude)  
- Repeated image inference (Replicate, Stability)  
- Exceeding platform quotas (Railway, Vercel)  

> Goal: Ensure your app remains free or low-cost until you're ready to scale.

---

## 10.2 Free Tier Comparison Recap (Limits)

|Platform	    |Monthly Free Tier	                |Key Limitations                        |
|---------------|-----------------------------------|---------------------------------------|
|OpenAI	        |Free trial ($5–$18, one-time)	    |After trial, pay-per-token             |
|Replicate	    |$10 in credits (one-time)	        |Pay-per-inference after                |
|Hugging Face	|Free Spaces + CPU only	            |No GPU, low RAM unless PRO             |
|Railway	    |500 hrs/month, 1GB deploy	        |Cold starts, no GPU                    |
|Vercel	        |100GB bandwidth, unlimited deploys	|Bandwidth limit for image-heavy apps   |

> 💡 You can deploy multiple apps under one free plan — just rotate projects if needed!

---

## 10.3 Control API Costs with Smart Code

1. Set Prompt Length Limits (for OpenAI)
```python    
    if len(prompt) > 250:
        return {"error": "Prompt too long"}
```
2. Add a Global Cooldown (e.g. 10 seconds)

```python    
    import time
    last_used = 0
    def safe_generate(prompt):
        global last_used
        now = time.time()
        if now - last_used < 10:
            return {"error": "Please wait before generating again."}
        last_used = now
        # call your OpenAI API
```

3. Use Small Models First

|Model	                    |Est. Cost per 1K tokens    |
|---------------------------|---------------------------|
|gpt-3.5-turbo	            |~$0.0015                   |
|gpt-4	                    |~$0.03–$0.06               |
|openai/text-davinci-003	|~$0.02                     |

> Use gpt-3.5-turbo by default for text.

---

## 10.4 Optimize Image Inference (Replicate)

|Strategy	                    |Result                     |
|-------------------------------|---------------------------|
|Cache output URLs	            |Save storage and bandwidth |
|Avoid large image inputs	    |Resize before sending      |
|Use “Preview” mode in demos	|Lower-res output = cheaper |
|Bundle image post-processing	|Avoid second inference step|

> You can also pre-generate results for demos to avoid live inference costs.

---

## 10.5 Monitoring Usage & Logs

|Platform	            |Tool / Page	        |What to Check                          |
|-----------------------|-----------------------|---------------------------------------|
|Hugging Face	        |Spaces → Logs	        |Inference errors, load time            |
|Railway	            |Project Logs	        |Backend errors, API calls, cold starts |
|OpenAI	                |Usage Dashboard	    |Token usage and cost breakdown         |
|Replicate	            |Billing Page + History	|# of model runs, average cost          |
|Vercel	                |Analytics (Pro only)	|Bandwidth, requests                    |

> Check logs after every major feature update.

---

## 10.6 Add Logging to Your Backend

**utils/logger.py**
```python    
    import datetime
    def log_usage(endpoint: str, input_data: dict):
        with open("logs.txt", "a") as f:
            f.write(f"{datetime.datetime.now()} | {endpoint} | {input_data}\n")
```

In your API route:
```bash    
    log_usage("/generate", {"prompt": request.prompt})
```

---

## 10.7 Automation Tools (Advanced)

|Tool	            |Use Case                                       |
|-------------------|-----------------------------------------------|
|cron + curl	    |Auto-ping API to avoid cold starts (Railway)   |
|PostHog	        |Track frontend behavior/events                 |
|Sentry	            |Monitor frontend/backend errors                |
|Google Analytics	|Track public usage                             |

---

## 10.8 Final Cost-Safety Checklist

|Task	                            |Done?  |
|-----------------------------------|-------|
|Prompt/input length limited	    |✅     |
|Cooldown between API calls enforced|✅     |
|.env keys secured	                |✅     |
|Fallback error handling added	    |✅     |
|Logs monitored and reviewed weekly	|✅     |
|Platform usage dashboards checked	|✅     |

> Bonus: Add a Usage Warning in UI
```python
    if (usageCount >= 3) {
      alert("You’ve used 3/5 free generations. Upgrade for more!");
    }
```    
> Great for apps you plan to monetize later!

---

## Chapter Summary

- You now know how to control AI API costs using simple tricks

- You can monitor app health and usage through logs and dashboards

- Your app is now safe for public sharing or demoing

---



