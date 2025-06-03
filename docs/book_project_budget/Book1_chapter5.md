# 📘 Chapter 5: Building the Frontend UI


## 5.1 Choosing Your UI Strategy

|Option	                |Tech Stack	        |Best For	                        |Deployment     |
|-----------------------|-------------------|-----------------------------------|---------------|
|React (CRA or Vite)	|JS + JSX + Hooks	|Full UI customization + clean UX	|Vercel/Netlify |
|Gradio	                |Python	            |Fast ML prototyping & demo	        |Hugging Face   |
|Streamlit	            |Python	            |Interactive analytics dashboards	|Streamlit Cloud|

✅ For professional, portfolio-ready apps: Use React + Vercel
✅ For fast, model-focused demos: Use Gradio + Hugging Face

---

## 5.2 Setting Up a React Frontend (with API Integration)
**📂 Folder: frontend/**
```bash
    npx create-react-app frontend
    cd frontend
    npm install
```

**🌐 Add .env in frontend/**
```bash
    REACT_APP_API_URL=http://localhost:8000
    Make sure to restart npm start after editing .env.
```

---

## 5.3 Basic Frontend Example: Meme Generator UI

**📄 src/App.js**
```javascript
    import { useState } from "react";
    function App() {
      const [prompt, setPrompt] = useState("");
      const [caption, setCaption] = useState("");
      const handleGenerate = async () => {
        const response = await fetch(`${process.env.REACT_APP_API_URL}/generate`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ prompt }),
        });
        const data = await response.json();
        setCaption(data.caption);
      };
      return (
        <div style={{ padding: "2rem" }}>
          <h1>🧠 AI Meme Generator</h1>
          <input
            type="text"
            value={prompt}
            placeholder="Enter your meme idea..."
            onChange={(e) => setPrompt(e.target.value)}
            style={{ width: "300px", padding: "10px" }}
          />
          <button onClick={handleGenerate} style={{ marginLeft: "1rem" }}>
            Generate
          </button>
          {caption && (
            <div style={{ marginTop: "2rem", fontSize: "1.2rem" }}>
              <strong>Caption:</strong> {caption}
            </div>
          )}
        </div>
      );
    }
    export default App;
```

---

## 5.4 Testing Locally
**Run backend:**
```bash
    cd backend
    uvicorn app.main:app --reload
```
**Run frontend:**
```bash
    cd frontend
    npm start
```

> Make sure ports match. Use CORS middleware in backend to allow frontend access.

---

## 5.5 Optional: Add UI Enhancements

|Feature	            |Tools / Libraries	        |Notes                                  |
|-----------------------|---------------------------|---------------------------------------|
|Dark Mode Toggle	    |Tailwind / CSS Toggle	    |Use a button to switch themes          |
|Export CSV / Image	    |react-csv, html2canvas	    |Great for sentiment tools / charts     |
|Toast Notifications	|react-toastify	            |Nice user feedback                     |
|Loading Animation	    |react-loader-spinner	    |Show while model/API is running        |

---

## 5.6 Connecting to Gradio Instead (Alternative UI)

For rapid prototyping, use Gradio directly instead of building a frontend:
**📄 app.py**
```python
    import gradio as gr
    from cartoonize import cartoonize_image
    def run(image):
        return cartoonize_image(image)
    gr.Interface(fn=run, inputs="image", outputs="image").launch()
```

> Works beautifully in Hugging Face Spaces and is GPU-optimized if you upgrade.

---

## 5.7 Final Checklist Before Deployment

- Frontend fetches data from backend

- .env is used (no hardcoded URLs)

- UI shows prediction/result

- User input is validated

- Styles are mobile-friendly (CSS/Tailwind)

---

## Chapter Summary

- You’ve built a working frontend that connects to your backend API.

- You’re using .env to keep API calls dynamic.

- Your app is now user-ready — clean, interactive, and scalable!

---