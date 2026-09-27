# Project Context — Explainable AI Model Health Monitoring & Drift Diagnosis System

> This file is the single source of truth for the project's current state.
> Update it whenever a significant decision, milestone, or architectural change happens.
> It is used as the shared context when working with AI assistants.

---

## 1. Project Identity

**Full Title:** Explainable AI Model Health Monitoring and Drift Diagnosis System

**Alternative Names:**
- Explanation Drift Based Early Warning System
- Explainable Drift Detection and Recommendation System
- Model Health Doctor for AI Systems
- Explainable Model Monitoring and Recovery Framework

**Academic Context:**
- University: Savitribai Phule Pune University (SPPU)
- Program: Bachelor of Engineering (Computer Engineering)
- Team Size: 3 members
- Type: Final Year Engineering Project
- Current Phase: Research Gap Identification + Architecture Design + Prototype Setup

---

## 2. Core Vision

Existing ML monitoring tools detect drift, generate alerts, and provide SHAP explanations.
They do **not** answer:
1. Why exactly is the model degrading?
2. Has the model's reasoning changed?
3. How severe is the degradation?
4. What corrective action should be taken?
5. Can degradation be detected before accuracy drops?

**Our goal:** Build an intelligent model health monitoring system that:
- Detects drift
- Explains drift
- Quantifies explanation drift
- Estimates model health
- Diagnoses root causes
- Recommends corrective actions

**Primary Research Question:**
> Can explanation drift be used as an early warning signal for model degradation
> before traditional performance metrics begin to decline?

---

## 3. Key Contributions

1. **Explanation Drift Score (EDS)** — measures changes in model reasoning over time
   - Components under consideration: SHAP distribution shift, feature importance shift, feature ranking shift, Jensen-Shannon divergence, cosine similarity, Wasserstein distance

2. **Model Health Score (MHS)** — single interpretable 0–100 health score
   - Inputs: Data Drift Score (DDS), Explanation Drift Score (EDS), Prediction Stability (PS), Performance Trend (PT)
   - Conceptual formula: `MHS = 1 - (w1·DDS + w2·EDS + w3·(1-PS) + w4·(1-PT))`
   - Bands: 90–100 Healthy, 70–89 Stable, 50–69 Warning, 0–49 Critical

3. **Drift Diagnosis Engine** — identifies root causes (covariate shift, feature drift, class imbalance, distribution shift, concept drift)

4. **Adaptation Recommendation Engine** — recommends corrective actions (recommendation-only, **not autonomous retraining**)

5. **Early Warning System (stretch goal)** — validate that EDS rises before accuracy drops

---

## 4. Extended Workflow
```
Reference Dataset
↓
Train Model
↓
Generate Baseline Explanations
↓
Store Baseline Knowledge
↓
Incoming Production Data
↓
Data Drift Detection
↓
SHAP Analysis
↓
Explanation Drift Score (EDS)
↓
Model Health Score (MHS)
↓
Drift Diagnosis Engine
↓
Adaptation Recommendation Engine
↓
Desktop Dashboard
↓
Reports & Analytics
```

---

## 5. Technology Stack

**Desktop Shell:** Tauri (v2)

**Frontend:** React + TypeScript + Vite
**UI:** Tailwind CSS + shadcn/ui
**Charts:** Recharts (+ Plotly for Python-side)
**Data Fetching:** TanStack Query

**Backend API:** FastAPI (Python)

**ML / Data:**
- Scikit-Learn, XGBoost
- LightGBM (optional), PyTorch (future experiments)
- Pandas, NumPy

**Explainability:** SHAP (primary), LIME (optional)

**Drift Detection:** Evidently AI (primary), River (optional), Alibi Detect (optional)

**Database:** SQLite (dev) → PostgreSQL (future)
**ORM:** SQLAlchemy

**Auth (future):** JWT + FastAPI Security

**Dev Tools:** Git, GitHub, VS Code
**Docs:** Markdown, Mermaid, Draw.io

---

## 6. Datasets

**Primary:** Credit Card Fraud Detection (highly imbalanced, real-world)
**Secondary:** Loan Approval Dataset (easier interpretation, faster prototyping)

---

## 7. Application Modules (Planned)

1. Dashboard — Health score, drift status, alerts, trends
2. Model Monitoring — Model list, performance metrics, health history
3. Drift Monitoring — Data drift, feature drift, concept drift, reports
4. Explanation Drift Analysis — Baseline vs current explanations, feature importance changes, EDS
5. Diagnosis Engine — Root cause analysis, drift classification, severity
6. Recommendation Center — Suggested actions, recovery plans, mitigation strategies
7. Reports Module — PDF reports, health reports, drift reports
8. Settings — Model configurations, monitoring rules, threshold management

---

## 8. Differentiators vs Existing Tools

**Existing:** Evidently AI, Arize AI, Fiddler AI, NannyML
**What they offer:** Drift detection, monitoring, explainability

**Our differentiators:**
- Explanation Drift Score (EDS)
- Model Health Score (MHS)
- Drift Diagnosis Engine
- Adaptation Recommendation Engine
- Early Warning research
- Imbalanced data evaluation

**Critical constraint:**
The project MUST NOT become "Evidently + SHAP + Dashboard".
It must provide original methodology, research contribution, experimental evaluation, and publishable insights.

---

## 9. Architecture Decisions

### Desktop Application Decision
Chosen **Tauri** over Electron because:
- Lightweight, native performance
- Lower memory usage
- Production-style desktop app (not a simple academic dashboard)

### Backend Packaging Strategy
- Backend runs as a **Tauri sidecar** (child process) in production
- Compiled via **PyInstaller** into a standalone executable
- Placed in `src-tauri/binaries/` with target-triple suffix
- Bundled into the installer via `tauri.conf.json` `externalBin`

### Development vs Production
- **Development:** FastAPI and Tauri run separately (two terminals, HTTP on localhost)
- **Production:** PyInstaller-compiled backend + Tauri sidecar pattern → single installer

### Model Storage
Deep learning models that analyze the monitored model will be stored as **Tauri bundled resources** under `resources/models/` and declared in `tauri.conf.json` bundle section.
Rationale: Ready to use at install time, no network required for demos/evaluation.

---

## 10. Repository Layout
```
Explainable-Drift-Detection-Recommendation-System/
├── .github/
│ ├── pull_request_template.md
│ └── ISSUE_TEMPLATE/
│ ├── task.md
│ ├── bug.md
│ └── config.yml
├── backend/ ← FastAPI backend (skeleton pending)
├── docs/ ← Documentation, diagrams, papers
├── ExplainAI/ ← Tauri + React desktop app
│ ├── src/ ← React frontend
│ ├── src-tauri/ ← Tauri / Rust shell
│ ├── tailwind.config.js
│ ├── vite.config.ts
│ └── package.json
├── CONTEXT.md ← this file
├── .gitignore
├── .gitattributes
├── LICENSE
└── README.md
```

---

## 11. Repository Setup Status

| Item | Status |
|---|---|
| GitHub repo created | ✅ |
| Repo visibility | Private (will go public later) |
| Collaborators added | ❌ Not yet |
| Branch protection rule | ✅ Configured (activates when public) |
| Root `.gitignore` | ✅ |
| Tauri scaffold committed | ✅ |
| GitHub templates committed | ✅ |
| Backend skeleton | ❌ Not started |
| Frontend UI setup (Tailwind + shadcn) | ❌ Not started |
| PyInstaller sidecar setup | ❌ Not started |

---

## 12. Development Workflow

**Branch strategy:**
- `main` — protected, no direct pushes
- Feature branches: `feature/...`, `fix/...`, `docs/...`, `chore/...`

**PR flow:**
1. Pull latest `main`
2. Create feature branch
3. Commit and push
4. Open PR, request review
5. Teammate approves → merge

**Team split (tentative):**
- Member 1: Frontend (Tauri + React UI)
- Member 2: Backend (FastAPI + ML pipeline)
- Member 3: Research, evaluation, documentation

---

## 13. Current Next Steps

1. ✅ Set up repo and templates
2. ⏳ Finish backend skeleton so teammates can start in parallel
3. ⏳ Set up Tailwind + shadcn/ui for frontend development
4. ⏳ Validate research gap in writing
5. ⏳ Formalize EDS formula
6. ⏳ Formalize MHS formula
7. ⏳ Identify evaluation metrics
8. ⏳ Meet project coordinator
9. ⏳ Finalize topic

---

## 14. Long-Term Research Goals

Potential future extensions:
1. Semi-automatic adaptation
2. Drift prediction
3. Regression support
4. NLP drift detection
5. Image model drift detection
6. Real-time streaming data
7. Self-healing ML systems

---

**Last Updated:** 28 Sep 2026
**Maintained By:** Soham (repo owner)