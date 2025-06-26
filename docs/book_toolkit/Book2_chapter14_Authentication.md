---
hide:
  - toc
---

# Chapter 14: Authentication, Databases & User Management

*“Projects grow when users trust them.”*

This chapter takes us from solo demos to **real users**, **real data**, and **real structure**.
If your AI project is going to grow, it will need:

> ✅ Authentication
> ✅ A database
> ✅ User management

This is how you go from **fun app** to **platform**.

---

## This chapter covers:

* Why auth & user data matter in AI apps
* Supabase vs Firebase (and how to choose)
* Adding login, storing usage history
* Schema design for prediction-based apps
* Builder’s lens: building with identity in mind

---

## Opening Reflection: The Shift from Tools to Ecosystems

> *“When one user logs in… you’ve moved from app to experience.”*

An AI model can:

* Classify text
* Draw an image
* Predict stock prices

But without:

* A login button
* A way to store results
* A user dashboard

…it’s just a moment of interaction.

Once you add identity, you gain:

* ✅ Memory
* ✅ Personalization
* ✅ Security
* ✅ Community

This is how apps grow into **ecosystems**.

---

## 14.1 Why You Need Auth & Data

Even the simplest AI app benefits from:

| Feature        | Why It Matters                             |
| -------------- | ------------------------------------------ |
| Login          | Track who used what, when                  |
| History        | Store past predictions (e.g., memes, text) |
| Usage Tracking | Show stats (generations, liked results)    |
| Billing Tiers  | Limit features per user plan               |

---

## 14.2 Choosing Your Stack: Supabase vs Firebase

| Feature     | Supabase                          | Firebase                             |
| ----------- | --------------------------------- | ------------------------------------ |
| Language    | SQL/Postgres, JS/Python clients   | NoSQL/Firestore, JS-heavy            |
| Auth        | Email, OAuth, Magic Link          | Email, OAuth, Phone, Anonymous       |
| Realtime DB | ✅ Yes (Postgres pub/sub)          | ✅ Yes (Firestore)                    |
| Storage     | File storage (avatars, images)    | Cloud Storage                        |
| Open Source | ✅ Yes                             | ❌ Proprietary                        |
| Great For   | SQL lovers, devs who want control | Fast prototyping, Google integration |

**🔧 Recommendation for AI Builders:**
Use **Supabase** if you want SQL + auth + file storage all in one stack.

---

## 14.3 Adding Authentication (Supabase Example)

Install the Python client:

```bash
pip install supabase
```

Create a project → Get your `URL` and `anon/public API key`
[Sign up at https://supabase.com](https://supabase.com)

### Python Code

```python
from supabase import create_client

url = "https://xyzcompany.supabase.co"
key = "your-public-anon-key"
supabase = create_client(url, key)

# Sign up a new user
auth_response = supabase.auth.sign_up({
    "email": "user@example.com",
    "password": "strongpassword"
})
```

✅ Use frontend (React or Gradio) to call this via Supabase API or JS SDK.

---

## 14.4 Design Your Schema (Prediction-Based App)

### Table: `users`

| id | email                           | created\_at |
| -- | ------------------------------- | ----------- |
| 1  | [user@a.com](mailto:user@a.com) | 2025-04-23  |

### Table: `predictions`

| id | user\_id | input\_text         | result\_text | model\_used       | timestamp        |
| -- | -------- | ------------------- | ------------ | ----------------- | ---------------- |
| 1  | 1        | "Is this positive?" | "POSITIVE"   | bert-base-uncased | 2025-04-23 14:12 |

### Benefits:

* Track usage per user
* View prediction history
* Filter by model/task
* Export data for analytics

---

## 14.5 Frontend Integration (React or Gradio)

### Gradio Example:

```python
gr.Textbox(label="Username")
gr.Textbox(label="Password", type="password")
```

### React + Supabase/Firebase:

* Supabase JS SDK
* Firebase Auth
* Custom backend endpoints (`/login`, `/signup`)

Add these for production readiness:

* JWT tokens for session management
* LocalStorage for persistent login
* Role-based routing (e.g., `user`, `admin`)

---

## 14.6 Builder’s Lens: Identity Unlocks Continuity

> *“Without users, you have usage.
> With users, you have relationships.”*

When users log in:

* They expect quality
* They invest in results
* They complete the loop:
  `input → feedback → return`

This is how you build real AI products — not just demos.

---

## Summary Takeaways

| Concept               | Why It Matters                         |
| --------------------- | -------------------------------------- |
| Auth = ownership      | Ties data and actions to a real person |
| DB = memory           | Enables history, analytics, billing    |
| Supabase = full stack | SQL + auth + file store + REST API     |
| Identity = UX layer   | From toy → tool → trusted experience   |

---

## 🌟 Closing Reflection

> *“Every user login is a vote of trust.
> The database is where you honor it.”*

---