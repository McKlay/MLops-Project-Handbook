# 📘 Chapter 7: Backend Deployment Options (Free Tier Ready)

Chapter 7 is all about getting your AI backend online—so anyone in the world can use your model or app. We'll walk through free-tier deployment options like Hugging Face Spaces, Railway, and Render, and help you choose the one that fits your project best.

---

## 7.1 Before You Deploy — Checklist

✅ You should already have:

- /backend/app/main.py (your FastAPI logic or app.py for Gradio)

- requirements.txt with all dependencies

- .env (for local secrets)

- Dockerfile or a deployment config (optional)

We’ll now deploy this backend to one of these platforms:

---

## 7.2 Option A: Hugging Face Spaces (Gradio or FastAPI)

✅ Pros:

- Super beginner-friendly

- Great for demo apps or model showcases

- Free GPU (on PRO) or CPU (on free tier)

🚫 Cons:

- Limited memory (2–6 GB)

- CPU only unless upgraded

- Best for Gradio or lightweight FastAPI

FastAPI Setup on HF Spaces
**📄 app.py**
```python
   from fastapi import FastAPI
   app = FastAPI()
   @app.get("/")
   def root():
       return {"message": "Hello Hugging Face!"}
```
**📄 requirements.txt**
```bash
   fastapi
   uvicorn
   python-dotenv
   openai
```
**📄 README.md**
```java
   ---
   title: My AI API
   emoji: 🤖
   colorFrom: gray
   colorTo: indigo
   sdk: docker
   ---
   # My AI App
   An API powered by FastAPI and OpenAI!
```   
**📄 Dockerfile (if needed)**
```java   
   FROM python:3.10
   WORKDIR /app
   COPY . /app
   RUN pip install -r requirements.txt
   CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "7860"]
```
**Push to Hugging Face:**
```bash
   git init
   git remote add origin https://huggingface.co/spaces/your-username/your-space
   git add .
   git commit -m "initial commit"
   git push -u origin main
```
---

### 🛤️7.3 Option B: Railway (FastAPI + OpenAI + Replicate)

✅ Pros:

- Perfect for FastAPI-based backends

- Easy GitHub integration

- 500 free compute hours/month

🚫 Cons:

- Cold starts (10–30s delay)

- No GPU on free tier

- Can timeout on long API responses

**FastAPI Setup for Railway**  
**📄 backend/requirements.txt**
```bash
   fastapi
   uvicorn
   python-dotenv
   openai
   replicate
```

**📄 pyproject.toml** (optional if you need Railway to detect the start command)

```bash
   [tool.poetry]
   name = "myaiapp"
   version = "0.1.0"
   [tool.poetry.scripts]
   start = "uvicorn app.main:app --host 0.0.0.0 --port $PORT"
```

1. **Push Backend Repo to GitHub**
```bash
git init
git add .
git commit -m "ready for deployment"
git remote add origin https://github.com/<your-username>/<your-repo>
git push -u origin main
```

2. **Connect to Railway**  
   • Go to https://railway.app  
   • New Project → Deploy from GitHub  
   • Set environment variables:  
      &nbsp;&nbsp;&nbsp;&nbsp;○ OPENAI_API_KEY  
      &nbsp;&nbsp;&nbsp;&nbsp;○ REPLICATE_API_TOKEN  
   • Done ✅  

---

## 7.4 Option C: Render

✅ Pros:

- Simple, fast deploys

- Free up to 750 hrs/month

- Can run background tasks

🚫 Cons:

- Cold starts like Railway

- Slower startup than Railway
   
Basic Deploy Steps:

   1. Create backend repo → push to GitHub

   2. Go to https://render.com

   3. New → Web Service → Connect your GitHub repo

   4. Use uvicorn app.main:app --host 0.0.0.0 --port 10000 as Start Command

   5. Set environment variables

   6. Done ✅

---

## 7.5 Managing Environment Variables

|Platform	      |Where to Add Them                           |
|-----------------|--------------------------------------------|
|Hugging Face	   |*Settings > Secrets*                        |
|Railway	         |*Project > Variables*                       |
|Render	         |*Environment > Add Environment Variables*   |

Add:
```bash
   OPENAI_API_KEY=sk-xxxx
   REPLICATE_API_TOKEN=r8_xxxx
```

> 🚫 Don’t commit .env to GitHub — keep it local or use .gitignore.

---

## 7.6 Deployment Checklist

- Backend runs uvicorn app.main:app

- requirements.txt is complete

- GitHub repo is pushed

- Environment variables added

- Deployment platform is selected (HF, Railway, or Render)

- Test /generate or /predict endpoint with Postman or browser

## Chapter Summary

- You deployed your backend to the cloud!
- Hugging Face Spaces = great for demos
- Railway = great for FastAPI-powered APIs
- Render = great alternative with generous limits
- Your AI app is now globally accessible 🌐

---

