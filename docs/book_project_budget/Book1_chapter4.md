# 📘 Chapter 4: Building the ML Logic

## 4.1 Choose Your Model Strategy

There are two major approaches you can take depending on your goal and available compute:

|Strategy	                |Description                                                                                |
|---------------------------|-------------------------------------------------------------------------------------------|
|A. Local Pretrained Model	|Use a model like BERT, CartoonGAN, or Fast Style Transfer from transformers or PyTorch     |
|B. API-Driven Inference	|Use external services (OpenAI, Replicate) to run inference and return results              |

Let’s cover both methods, and you can pick which one fits each project.

---

## 4.2 Method A: Local Inference Using Pretrained Model

Perfect for small NLP tasks or lightweight image models.

**Example: Local Sentiment Classifier with BERT**

📂 backend/app/main.py

```python
from fastapi import FastAPI
from pydantic import BaseModel
from transformers import BertTokenizer, BertForSequenceClassification
import torch
# Load model + tokenizer
tokenizer = BertTokenizer.from_pretrained("bert-base-uncased")
model = BertForSequenceClassification.from_pretrained("nlptown/bert-base-multilingual-uncased-sentiment")
app = FastAPI()
class InputText(BaseModel):
    text: str
@app.post("/predict")
def predict_sentiment(input: InputText):
    inputs = tokenizer(input.text, return_tensors="pt", truncation=True, padding=True)
    outputs = model(**inputs)
    probs = torch.nn.functional.softmax(outputs.logits, dim=1)
    label = torch.argmax(probs).item()
    return {"label": label, "confidence": round(probs[0][label].item(), 4)}
```

> Good for NLP-based tools like Sentiment Analyzer, News Classifier, etc.

---

## 4.3 Method B: API-Based Inference (e.g., OpenAI, Replicate)

This is best for:

- Projects with limited local compute.

- Tasks like GPT chat, DALL·E image generation, image-to-image style transfer.

**Example: GPT-based Caption Generator (OpenAI)**
📂 backend/app/main.py
```python
import os
import openai
from dotenv import load_dotenv
from fastapi import FastAPI
from pydantic import BaseModel
load_dotenv()
openai.api_key = os.getenv("OPENAI_API_KEY")
app = FastAPI()
class PromptInput(BaseModel):
    prompt: str
@app.post("/generate")
def generate_caption(input: PromptInput):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a witty meme caption generator."},
            {"role": "user", "content": input.prompt}
        ]
    )
    caption = response['choices'][0]['message']['content']
    return {"caption": caption}
```

**🖼️ Example: CartoonGAN via Replicate API**
📂 backend/app/model/cartoonize.py

```python
import replicate
import os
replicate.Client(api_token=os.getenv("REPLICATE_API_TOKEN"))
def cartoonize_image(image_path: str):
    output = replicate.run(
        "tstramer/cartoonify:latest",
        input={"image": open(image_path, "rb")}
    )
    return output  # typically a URL to the generated image
```

---

## 4.4 Handling Inference Responsibly

|Concern	             |Solution                                                  |
|------------------------|----------------------------------------------------------|
|🛑 Timeouts	        |Add try/except, request timeouts (especially for APIs)     |
|🧬 Reproducibility	    |Set random seeds, save versions of models                  |
|🔐 API Key Safety	    |Use .env, never hardcode keys                              |
|💸 Cost Management	    |Throttle usage (e.g., max 3 calls/min/user)                |
|🚫 Bad Inputs	        |Sanitize user input to avoid prompt injection              |

---

## 4.5 Local Testing

Before you deploy, run local tests using:

```python
uvicorn app.main:app --reload
```

Then test with:

```python
curl -X POST "http://localhost:8000/generate" \
     -H "Content-Type: application/json" \
     -d "{\"prompt\":\"Make a funny caption for a dog eating pizza.\"}"
```

Or use Postman / Thunder Client (VSCode plugin) for easier testing.

---

## 4.6 Recap: What You’ve Accomplished

- Built model inference logic (either locally or via API).

- Secured your keys and API calls.

- Tested that it responds to user input.

- Ready to connect to your frontend.

**Bonus (Optional): Add CORS if Frontend Can’t Reach Backend**
```python
from fastapi.middleware.cors import CORSMiddleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # In production, set this to your frontend URL
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

