---
hide:
  - toc
---

# Chapter 8: Frontend Deployment (Vercel, Netlify)

Chapter 8 is where we make your frontend live, hosted on platforms like Vercel or Netlify, and connected to your deployed backend (Railway, Hugging Face, or Render). Once done, your project will be accessible worldwide.

---

## 8.1 Choose the Right Platform

|Platform	    |Best For	                |CLI Tool	    |Free Tier Includes                     |
|---------------|---------------------------|---------------|---------------------------------------|
|Vercel	        |React, Vite, Next.js apps	|vercel	        |100GB bandwidth, 100 deployments/mo    |
|Netlify	    |Static sites (React, HTML)	|netlify	    |100GB bandwidth, Forms, Edge Functions |

> Recommendation: Vercel for React projects — seamless with GitHub, supports environment variables.

---

## 8.2 Preparing Your React Frontend for Deployment
Folder: frontend/
Structure should look like this:
```bash
    frontend/
    ├── public/
    ├── src/
    ├── .env               # for API endpoint
    ├── package.json
    └── README.md
```
Sample .env:
```bash
REACT_APP_API_URL=https://my-railway-backend.up.railway.app
```
Sample fetch in App.js:
```java
    fetch(`${process.env.REACT_APP_API_URL}/generate`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ prompt }),
    })
```

---

## 8.3 Deployment to Vercel (Step-by-Step)
Prerequisites:

- A GitHub account

- Frontend project pushed to GitHub

Deployment Process:  
    1. Go to https://vercel.com  
    2. Click "New Project" → Import from GitHub  
    3. Select your frontend repo  
    4. Set Environment Variables (from .env):  
    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;REACT_APP_API_URL=https://your-backend-url
    
    5. Click Deploy — and you’re done 🎉

Build command (auto-detected):
```bash    
    npm run build
```

---

## 8.4 Deployment to Netlify (Alternative Option)

```bash    
    npm install -g netlify-cli
    netlify login
```

Deploy:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netlify init  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netlify deploy --prod
    
You’ll be asked for:
&nbsp;&nbsp;&nbsp;&nbsp;○ Build directory → build/  
&nbsp;&nbsp;&nbsp;&nbsp;○ Environment variables → Set them in Netlify UI

---

## 8.5 Environment Variable Tips

|Variable Name	        |Description	                    |Where to Add                               |
|-----------------------|-----------------------------------|-------------------------------------------|
|REACT_APP_API_URL	    |Backend API base URL	            |Vercel / Netlify → Settings → Environment  |
|NODE_ENV=production	|Optional flag to optimize builds	|.env or host dashboard                     |

> Don’t hardcode backend URLs directly into code. Use .env.

---\

## 8.6 Final Checklist for Production

|Checkpoint	                                |Done?  |
|-------------------------------------------|-------|
|Frontend builds successfully	            |✅     |
|Backend URL reachable (from browser)	    |✅     |
|API keys not exposed in frontend	        |✅     |
|Environment variables set in Vercel	    |✅     |
|Mobile/desktop responsive	                |✅     |
|HTTPS (secured) deployment	                |✅     |

> Bonus: Adding a Custom Domain

Vercel and Netlify both support free subdomains (like myapp.vercel.app), but you can:

- Add a custom domain (like claylabs.ai)

- Use Vercel/Netlify DNS to manage the domain

It’s optional, but adds professional polish

---

## Chapter Summary

- You’ve deployed your frontend (React or Gradio) to a public URL.

- You’re using .env to connect safely to your backend API.

- Your AI/ML project is now globally available and production-ready.

---
