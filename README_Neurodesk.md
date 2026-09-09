# 🧠 NeuroDesk — AI-Powered Productivity Chrome Extension
### AI Spam Detection · Email Summarization · AI Chatbot · Smart Sticky Notes · Pomodoro · Reminders

![Chrome Extension](https://img.shields.io/badge/Chrome-Extension%20MV3-blue?logo=googlechrome)
![Backend](https://img.shields.io/badge/Backend-Python%20Flask-green?logo=python)
![AI](https://img.shields.io/badge/AI-Groq%20llama--3.3--70b-purple?logo=openai)
![ML](https://img.shields.io/badge/ML-Naive%20Bayes%20%2B%20TF--IDF-orange)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## 🎯 CSAR Framework Overview

| | |
|---|---|
| **Challenge** | Knowledge workers lose hours daily to inbox overload: spam slipping past filters, long emails demanding manual triage, and context scattered across tabs — with no unified, privacy-respecting AI assistant living inside the browser. |
| **Situation** | Gmail's native tooling offers no on-page AI spam scoring, no one-click summarization, and no persistent cross-tab note system. Third-party tools typically route private email content through external servers, creating privacy risk. |
| **Action** | Designed and shipped a **full-stack Chrome Extension (Manifest V3) + Python Flask backend**: an ML spam classifier (**Naive Bayes + TF-IDF, Scikit-Learn**) with an **active-learning feedback loop**, LLM-powered summarization & chatbot (**Groq llama-3.3-70b**), cross-tab synced sticky notes, Pomodoro timer, and reminders — wired together through 5 REST API endpoints. |
| **Result** | A production-grade, **9-feature** productivity suite that runs **locally and privacy-first** (email analysis happens on the user's own machine; notes never leave `chrome.storage.local`), with real-time Gmail UI injection, instant cross-tab note sync, and a zero-build-step vanilla-JS front end. |

---

## 🔴 The Challenge

Build an AI assistant that lives *inside* the user's workflow — not in another tab. Concretely:

1. **Real-time spam intelligence** on the Gmail page itself, not in a separate app.
2. **Instant comprehension** — one-click bullet-point summaries of long emails.
3. **Persistent memory** — notes that survive tab switches, reloads, and browser restarts, synced across every open tab.
4. **Privacy by design** — sensitive email content must be processed on the user's own backend, not shipped to third parties by default.
5. **Zero-friction install** — no npm, no build pipeline; load-and-go.

---

## 🟠 The Situation

The project targets the Chrome browser as the primary work environment and Gmail as the primary communication surface. Key constraints shaping the design:

- **Manifest V3** service-worker architecture (modern Chrome extension standard).
- A **local-first backend**: the Flask server runs on `localhost:5000`, so spam classification and data storage remain entirely under the user's control.
- **LLM access without infrastructure cost**: Groq's free tier provides `llama-3.3-70b-versatile` inference with generous rate limits — called only when the user explicitly triggers summarization or chat.
- **Front end in pure vanilla JavaScript** — no frameworks, no bundlers, no build step — maximizing portability and minimizing attack surface.

---

## 🟡 The Action

### 🏗️ Architecture — Full-Stack, 3-Layer Design

```
┌─────────────────────┐         ┌──────────────────────────────────┐
│   Chrome Extension  │         │   Flask Backend (localhost:5000) │
│                     │  REST   │                                  │
│  content.js ─ Gmail │────────▶│  /predict   → Spam detection     │
│  sticky.js ─ Notes, │         │  /summarize → Email summary      │
│             Chat,   │         │  /chat      → AI chatbot         │
│             Pomodoro│         │  /report    → Active learning    │
│  background.js ─    │         │  /health    → Status check       │
│    Service Worker,  │         │                                  │
│    Alarms, API Proxy│         │  train_model.py → NB + TF-IDF    │
└─────────────────────┘         └──────────────────────────────────┘
```

### ⭐ 9 Features Delivered

| Feature | Description |
|---|---|
| 🤖 **AI Spam Detection** | ML-powered (Naive Bayes + TF-IDF) real-time Gmail spam classifier with an **active-learning** feedback loop via the `/report` endpoint |
| 📝 **AI Email Summarizer** | One-click bullet-point summaries using **Groq llama-3.3-70b** |
| 💬 **AI Chatbot (NeuroDesk AI)** | Floating sidebar assistant for productivity help, Q&A, and email tips |
| 📌 **Smart Sticky Notes** | Draggable, resizable, **cross-tab persistent** notes with 4 premium dark-glass themes (Violet, Rose, Ocean, Forest) |
| 🍅 **Pomodoro Timer** | Focus timer with pause/resume, work/break switching, and session tracking |
| ⏰ **Reminders** | Persistent `chrome.alarms` reminders with full-screen notification animations |
| ✅ **Checklists** | Interactive to-do checklists embedded inside sticky notes |
| 📥 **Note Export** | Download any note as a `.txt` file |
| 🔄 **Cross-Tab Sync** | Notes appear instantly on all tabs — no refresh required |

### ⚙️ Engineering Highlights
- **Active Learning Loop**: user spam/ham reports feed back through `/report`, continuously improving the classifier post-deployment.
- **Service-Worker Proxy Pattern**: `background.js` centralizes API calls, alarms, and cross-tab state sync — MV3-compliant.
- **Automated Onboarding**: `setup.bat` creates the virtual environment, installs dependencies, generates `.env` from template, and **trains the spam model** in one step.
- **Privacy-First Data Flow**: classification runs locally; Groq is contacted only for explicitly triggered summarize/chat actions; notes stored exclusively in `chrome.storage.local`.

---

## 🟢 The Results

### Delivered Outcomes
- 🚀 **Shipped a 9-feature, production-grade Chrome Extension + Flask backend** — a complete full-stack product, not a script.
- 🧪 **Hybrid AI strategy**: classical ML (Naive Bayes + TF-IDF, locally trained with Scikit-Learn) for latency- and privacy-sensitive spam detection, + LLM (llama-3.3-70b via Groq) for generative tasks — the right model for each job.
- 🔁 **Self-improving system**: active-learning endpoint lets the spam model learn from user corrections after deployment.
- ⚡ **Zero-build front end**: pure vanilla JS — no npm, no bundler — load-unpacked and run.
- 🔒 **Privacy architecture**: all email analysis local; notes never leave the browser; third-party calls only on explicit user action.

### Impact
NeuroDesk consolidates spam triage, summarization, note-taking, focus timing, and reminders into a single in-browser AI workspace — eliminating context-switching and demonstrating end-to-end product engineering: **ML model training, REST API design, browser-extension architecture, LLM integration, and UX design** in one cohesive system.

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| Front End | Vanilla JavaScript (Chrome Extension, Manifest V3) |
| Back End | Python, Flask, Flask-CORS |
| Machine Learning | Scikit-Learn — Naive Bayes + TF-IDF |
| LLM | Groq API — `llama-3.3-70b-versatile` |
| Data | Pandas, python-dotenv |
| Browser APIs | `chrome.storage.local`, `chrome.alarms`, Service Workers |

**Dependencies**: `flask, flask-cors, pandas, scikit-learn, groq, python-dotenv`

---

## 🚀 Quick Start

### 1. Backend
```bash
cd backend
setup.bat            # Windows: venv + deps + .env + model training (first time only)
# Add your free key from https://console.groq.com to backend/.env:
#   GROQ_API_KEY=your_groq_api_key_here
start.bat            # or: python app.py  →  http://127.0.0.1:5000
```

### 2. Extension
1. Open Chrome → `chrome://extensions`
2. Enable **Developer Mode**
3. **Load unpacked** → select the `extension/` folder
4. Pin **NeuroDesk** from the toolbar

### 3. Use
Open any Gmail email (AI spam badge appears automatically) · click **Summarize** · create notes from the FAB button · open NeuroDesk AI Chat · set Pomodoros and reminders.

---

## 📁 Project Structure
```
 extension/          # Chrome Extension (MV3)
    manifest.json   # Extension config
    content.js      # Gmail spam detection & UI injection
    sticky.js       # Notes, chatbot, Pomodoro, reminders
    background.js   # Service worker: alarms, sync, API proxy
 backend/            # Python Flask API
    app.py          # All endpoints (/predict /summarize /chat /report /health)
    train_model.py  # Naive Bayes + TF-IDF training
    setup.bat / start.bat / .env.example
```

---

## 📝 ATS Keywords
`Chrome Extension Development` `Manifest V3` `Full-Stack Development` `Python` `Flask` `REST API` `JavaScript` `Machine Learning` `Naive Bayes` `TF-IDF` `Scikit-Learn` `Natural Language Processing` `NLP` `LLM Integration` `Groq API` `Active Learning` `Spam Classification` `AI Chatbot` `Text Summarization` `Service Workers` `Browser APIs` `Privacy-First Design` `Pandas`

## 💼 CV-Ready Bullets
- Designed and shipped **NeuroDesk**, a full-stack AI Chrome Extension (Manifest V3) + Python Flask backend delivering 9 productivity features, including ML-powered Gmail spam detection, LLM email summarization, an AI chatbot, and cross-tab synced sticky notes.
- Built a **Naive Bayes + TF-IDF spam classifier** (Scikit-Learn) with an **active-learning feedback loop** that improves the model from user corrections post-deployment, exposed through 5 REST API endpoints.
- Integrated **Groq's llama-3.3-70b** for one-click email summarization and conversational assistance, with a **privacy-first architecture** — all email analysis runs locally and notes never leave the browser.
- Engineered a zero-build vanilla-JS front end with Gmail UI injection, service-worker API proxying, `chrome.alarms`-based reminders, and instant cross-tab state synchronization.

---

📄 **License:** MIT — free to use, modify, and distribute.
*Built with NeuroDesk AI*
