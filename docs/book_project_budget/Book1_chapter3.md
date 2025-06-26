---
hide:
  - toc
---

# Chapter 3: Setting Up Your AI/ML Project Repository

## 3.1 Why Structure Matters

Before you write a single line of code, a well-organized project structure will help you:

- Avoid messy code and deployment chaos.

- Easily integrate APIs, models, and UIs.

- Keep frontend/backend separate for clean DevOps.

- Make it GitHub-ready and team-collaboration friendly.

- Smoothly deploy on Hugging Face, Railway, or Vercel.

Goal: Create a modular structure that can be reused for any of your future AI/ML projects: Chatbots, Meme Generators, Cartoonizers, etc.

---

## 3.2 Recommended Folder Structure

**For Fullstack AI Projects (Frontend + Backend + Model/API):**
```bash
your_project/
│
├── frontend/                # React or Gradio UI
│   ├── public/
│   ├── src/
│   ├── .env                 # API_URL for backend
│   └── package.json
│
├── backend/                 # FastAPI or Flask logic
│   ├── app/
│   │   ├── main.py          # Main API logic
│   │   ├── model/           # Pretrained model / inference
│   │   └── utils.py         # Preprocessing, postprocessing
│   ├── .env                 # API keys (OpenAI, Replicate)
│   ├── requirements.txt
│   └── Dockerfile           # Optional for deployment
│
├── README.md                # Project overview
├── .gitignore               # Avoid committing secrets / venv
└── railway.toml             # If deploying to Railway (optional)
```

**For Gradio-only apps, you can simplify this to:**
```bash
your_project/
├── app.py
├── model/
├── assets/
├── requirements.txt
 └── README.md
```

---

## 3.3 Initializing the Project (Backend First)

```python
mkdir your_project && cd your_project
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install fastapi uvicorn transformers openai python-dotenv
pip freeze > requirements.txt
```

**Inside backend/app/main.py:**

```python
from fastapi import FastAPI
from pydantic import BaseModel
import os
from dotenv import load_dotenv
load_dotenv()
API_KEY = os.getenv("OPENAI_API_KEY")
app = FastAPI()
class Request(BaseModel):
    prompt: str
@app.post("/generate")
def generate(request: Request):
    # Your inference logic using API or model
    return {"output": f"Received: {request.prompt}"}
```

---

## 3.4 Setting Up the Frontend (React Example)

```bash
  cd frontend
  npx create-react-app .
  npm install
```

**Update .env in frontend:**
```python
  REACT_APP_API_URL=https://your-backend-api-url.com
```

**Update App.js to call backend:**
```python
fetch(`${process.env.REACT_APP_API_URL}/generate`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ prompt: userInput }),
})
  .then((res) => res.json())
  .then((data) => setResponse(data.output));
```

---

## 3.5 Managing Secrets Safely

**backend/.env**
```python
  OPENAI_API_KEY=sk-xxxxxxxxxx
  REPLICATE_API_TOKEN=r8_abc...
```

**.gitignore**
```python
  venv/
  __pycache__/
  *.pyc
  .env
  frontend/node_modules/
  frontend/.env
```

> Never push .env or .safetensors to GitHub. Use Hugging Face secrets or Railway dashboard instead for deployments.

---

## 3.6 Preparing for Deployment

  |Platform	          |Setup Step	                  |Tip                                                  |
  |-------------------|-----------------------------|-----------------------------------------------------|
  |Hugging Face	      |Push to main.py or app.py	  |Use gr.Interface() or FastAPI                        |
  |Railway	          |Connect repo, auto-deploy	  |Add start="uvicorn app.main:app" in pyproject.toml   |
  |Vercel (frontend)	|Link GitHub → Auto deploy	  |Don’t forget .env for API URL                        |

---

## 3.7 Checklist: Ready for GitHub + Deployment?

- Clean folder structure

- Working backend (/generate, /predict)

- API key hidden in .env

- Frontend calling backend with fetch()

- .gitignore in place

- README.md added

- Pushed to GitHub

---

## Chapter Summary

By the end of this chapter, your project is:

- Organized and modular.

- Ready for local testing and frontend integration.

- Protected from API key leaks.

- Just a few commands away from being deployed!

---

