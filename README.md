# HealthAssist AI 🩺🤖

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688.svg)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18-61DAFB.svg)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-5-646CFF.svg)](https://vitejs.dev/)
[![LangGraph](https://img.shields.io/badge/LangGraph-Multi--Agent-FF6F00.svg)](https://www.langchain.com/langgraph)

> **AI-powered healthcare triage, lab report analysis, and medication safety assistance system.**

A full-stack intelligent health assistant web application designed to help patients understand symptoms, interpret lab reports, identify potential risks, and receive personalized medication safety reviews and dietary schedules before consulting a physician.

---

> ⚠️ **Medical Disclaimer**  
> **HealthAssist AI is an educational and clinical decision-support tool, NOT a diagnostic medical device or a substitute for professional medical advice, diagnosis, or treatment.** Always seek the advice of a qualified physician or healthcare provider with any questions you may have regarding a medical condition. In case of an emergency in India, immediately dial **112** or **108** (or your local emergency services).

---

## 🌟 Key Features

| Feature | Description |
|---|---|
| 👤 **Encrypted Patient Profile** | Securely stores demographic data, chronic conditions, allergies, current medications, dietary preferences, and emergency contacts using AES Fernet field encryption. |
| ❓ **Adaptive AI Follow-Up** | Context-aware dynamic follow-up questioning (1–3 targeted questions at a time) powered by Gemini / Groq to gather crucial clinical details. |
| 📄 **Lab Report OCR & Flagging** | Supports PDF and image uploads of lab reports. Extracts text via Tesseract OCR / PyMuPDF and automatically flags biomarkers (Hb, Platelets, TSH, etc.) as Low, Normal, High, or Critical. |
| 📚 **Medical Knowledge Base (RAG)** | Grounded in curated medical references and local clinical documents indexed in ChromaDB vector storage with source attribution. |
| 🤖 **Multi-Agent Orchestration** | 7 specialized agents coordinated via LangGraph: Symptom, Report, Knowledge, Orchestrator, Risk, Medication, and Diet. |
| 💊 **Medication Safety & Timetable** | Validates medications against patient allergies, contraindications, and drug interactions from curated clinical databases, generating an automated daily dosing timetable. |
| 🥗 **Personalized 7-Day Diet Plan** | Tailored meal schedules (Indian Vegetarian & Non-Vegetarian) respecting allergens and health conditions, plus a natural remedy/no-medication option for mild cases. |
| 💰 **Live Pharmacy Price Comparison** | Real-time comparative pricing per tablet across leading Indian online pharmacies (PharmEasy, Netmeds, Tata 1mg, Apollo). |
| 🚨 **Emergency Triage Protocol** | Automatic red-flag detection (chest pain, stroke signs, severe dyspnea, critical lab values) triggering instant first-aid guidance and direct emergency dialers. |
| 📑 **Downloadable Clinical PDF** | Generates a clean, comprehensive clinical summary report for physician review. |
| 🔒 **Enterprise-Grade Security** | bcrypt password hashing, expirable JWT tokens, account lockout protection, HTTP security headers, and encrypted storage. |

---

## 🏗️ Architecture & Workflow

HealthAssist AI combines strict deterministic safety guardrails with generative AI intelligence:

```
[ Patient Input ] 
       │
       ├── Profile & Symptoms
       │
       ▼
[ Adaptive Triage Engine ] ────► Danger Sign Detected? ──► [ Emergency First-Aid Screen ]
       │                                                              (112 / 108 Call)
       ▼ (No Emergency)
[ Dynamic Follow-Up Questions ]
       │
       ▼
[ Optional Lab Report Upload ] ──► [ OCR Extraction & Clinical Range Flagging ]
       │
       ▼
┌────────────────────────────────────────────────────────┐
│             LangGraph Multi-Agent System               │
│                                                        │
│  ┌─────────────────┐       ┌────────────────────────┐  │
│  │  Symptom Agent  │       │      Report Agent      │  │
│  └────────┬────────┘       └───────────┬────────────┘  │
│           │                            │               │
│           ▼                            ▼               │
│  ┌──────────────────────────────────────────────────┐  │
│  │           Knowledge Agent (ChromaDB RAG)         │  │
│  └────────────────────────┬─────────────────────────┘  │
│                           ▼                            │
│  ┌──────────────────────────────────────────────────┐  │
│  │         Orchestrator & Risk Scoring Agent        │  │
│  └────────┬─────────────────────────────────────────┘  │
│           │                                            │
│           ├────────────────────────────┐               │
│           ▼                            ▼               │
│  ┌───────────────────┐       ┌──────────────────────┐  │
│  │ Medication Agent  │       │      Diet Agent      │  │
│  └───────────────────┘       └──────────────────────┘  │
└────────────────────────────────────────────────────────┘
       │
       ▼
[ Unified Health Report & Timetable ]
       ├── 7-Day Diet Schedule
       ├── Medicine Safety & Schedule
       ├── Pharmacy Price Comparison
       └── Exportable PDF Report
```

> **Safety by Design**: Emergency detection, lab range checks, drug interactions, and allergy exclusions are executed deterministically in code. AI enhances natural-language questions, reasoning summaries, and synthesis within validated Pydantic schemas.

---

## 💻 Tech Stack

- **Backend:** Python 3.11+, FastAPI, SQLAlchemy, SQLite / PostgreSQL, Pydantic, LangGraph, LangChain, ChromaDB, Google Gemini API / Groq API, PyMuPDF, Tesseract OCR, ReportLab, cryptography (Fernet), PyJWT, passlib (bcrypt).
- **Frontend:** React 18, Vite, React Router DOM, Modern CSS3.
- **Tooling:** Pytest, Uvicorn.

---

## 📁 Project Structure

```
HEALTH-ASSISTANT-AI/
├── backend/
│   ├── app/
│   │   ├── routers/          # FastAPI routes: auth, cases
│   │   ├── agents.py         # LangGraph multi-agent implementation
│   │   ├── config.py         # Application settings & environment variables
│   │   ├── db.py             # Database engine & session setup
│   │   ├── diet.py           # 7-day diet generation and nutritional rules
│   │   ├── emergency.py      # Emergency red-flag detection and emergency guides
│   │   ├── knowledge.py      # Curated medical knowledge indices
│   │   ├── labs.py           # Lab report OCR parsing and biomarker flagging
│   │   ├── llm.py            # Gemini / Groq LLM integration & fallback engine
│   │   ├── main.py           # FastAPI entrypoint & static frontend server
│   │   ├── medication.py     # Drug interaction and allergy validation
│   │   ├── models.py         # SQLAlchemy ORM models
│   │   ├── pdf_report.py     # PDF generation engine (ReportLab)
│   │   ├── pricing.py        # Pharmacy price scraping & normalization
│   │   ├── rag.py            # ChromaDB vector retrieval & warm-up
│   │   ├── schemas.py        # Pydantic validation schemas
│   │   ├── security.py       # Authentication, password hashing & tokens
│   │   └── triage.py         # Symptom triage logic
│   ├── data/                 # Curated medical data (drugs, conditions, lab ranges, foods)
│   ├── demo/                 # Sample CBC lab report for testing
│   ├── knowledge_base/       # Additional reference documents for RAG indexing
│   ├── scripts/              # Utility scripts (make_demo.py, import_prices.py)
│   ├── tests/                # Automated unit and API test suite
│   ├── requirements.txt      # Python dependencies
│   └── .env.example          # Environment variables template
├── frontend/
│   ├── src/
│   │   ├── components/       # UI components (DietChart, ResultView, PriceCompare, etc.)
│   │   ├── pages/            # Page views (Dashboard, CaseFlow, Profile, Login, etc.)
│   │   ├── api.js            # Axios/Fetch API client
│   │   ├── App.jsx           # Root React router component
│   │   └── styles.css        # Responsive styling
│   ├── package.json          # Node dependencies and build scripts
│   └── vite.config.js        # Vite configuration
├── dev.bat                   # Development launcher (auto-reloading API & Vite dev server)
├── setup.bat                 # One-click Windows setup script
├── start.bat                 # Production launcher (serves API + compiled React SPA)
├── SETUP_GUIDE.md            # Detailed end-user setup documentation
├── LICENSE                   # MIT License
└── README.md                 # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites

- **Python 3.11+**: [Download Python](https://www.python.org/downloads/) *(Ensure "Add python.exe to PATH" is checked)*
- **Node.js (LTS)**: [Download Node.js](https://nodejs.org/)
- **Tesseract OCR** *(Optional, for OCR on image lab reports)*: [UB-Mannheim Tesseract](https://github.com/UB-Mannheim/tesseract/wiki)
- **Google Gemini API Key**: Get a free API key from [Google AI Studio](https://aistudio.google.com/apikey)

---

### Windows Quick Start (Recommended)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/lalit-mittal49/health-assistant-ai.git
   cd health-assistant-ai
   ```

2. **Run setup:**
   Double-click `setup.bat` (or run in terminal):
   ```cmd
   setup.bat
   ```
   *This creates the Python virtual environment, installs backend dependencies, configures `.env`, and installs & builds the frontend.*

3. **Configure your API Key:**
   Open `backend/.env` and paste your Gemini API key:
   ```env
   GEMINI_API_KEY=your_gemini_api_key_here
   ```
   *(Optional fallback: add `GROQ_API_KEY=your_groq_key`)*

4. **Launch the application:**
   Double-click `start.bat`:
   ```cmd
   start.bat
   ```
   *Your browser will automatically open [http://127.0.0.1:8000](http://127.0.0.1:8000).*

---

### Manual / Cross-Platform Setup (Linux / macOS / Windows)

#### 1. Backend Setup

```bash
cd backend
python -m venv .venv

# Activate virtual environment:
# Linux/macOS:
source .venv/bin/activate
# Windows:
.venv\Scripts\activate

pip install -r requirements.txt
cp .env.example .env
# Edit .env and insert your GEMINI_API_KEY
```

#### 2. Frontend Setup

```bash
cd ../frontend
npm install
npm run build
```

#### 3. Run the Server

```bash
cd ../backend
uvicorn app.main:app --host 127.0.0.1 --port 8000
```
Open **http://127.0.0.1:8000** in your browser.

---

### Development Mode (Hot Reloading)

To run backend and frontend independently with auto-reload:

```bash
# Terminal 1: Backend
cd backend
uvicorn app.main:app --reload --port 8000

# Terminal 2: Frontend
cd frontend
npm run dev
```
Access the Vite dev server at **http://localhost:5173**.

---

## 🧪 Testing

Run automated backend tests using pytest:

```bash
cd backend
.venv\Scripts\python -m pytest tests -q
```

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.
