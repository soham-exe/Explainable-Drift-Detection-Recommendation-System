# Project Context — Explanation Drift-Based Early Warning and Diagnosis Framework

> This file is the single source of truth for the project's current state.
> Update it whenever a significant decision, milestone, or architectural change happens.
> It is used as the shared context when working with AI assistants.

---

## 1. Project Identity

**Working Title (during development):**
Explainable AI Model Health Monitoring and Drift Diagnosis System

**Target Title (if hypotheses H1/H2 validate):**
Explanation Drift-Based Early Warning and Diagnosis Framework for Machine Learning Models

> Rationale: The working title describes what we are building. The target title
> describes what we aim to prove. We only adopt the target title after
> experiments confirm the early-warning hypothesis. The title must be earned
> by results, not assumed.

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
- Current Phase: Phase 1 — Research Definition

---

## 2. Core Vision

Existing ML monitoring tools detect drift, generate alerts, and provide SHAP
explanations. They do **not** answer:
1. Why exactly is the model degrading?
2. Has the model's reasoning changed?
3. How severe is the degradation?
4. What corrective action should be taken?
5. Can degradation be detected before accuracy drops?

**Our thesis:**
Explanation drift — measurable changes in how a model reasons — may serve
as an early warning signal for model degradation, appearing before
traditional performance metrics decline.

**Primary Research Question:**
> Can explanation drift be used as an early warning signal for model
> degradation before traditional performance metrics begin to decline?

---

## 3. Hypotheses

These are the claims the project aims to test experimentally.
All contributions and metrics exist to support or falsify these hypotheses.

### H1 — Early Warning
> EDS increases before performance degradation under controlled
> drift scenarios.

**Test:** Inject drift, plot EDS vs. accuracy over time, check whether EDS
crosses its threshold before accuracy crosses its own.

**Status:** Untested.

### H2 — Superiority over Traditional Drift Metrics
> EDS provides earlier warning signals than traditional drift metrics
> (DDS-only monitoring).

**Test:** Run identical drift scenarios, measure lead time from DDS-only
monitoring vs. EDS-inclusive monitoring, compare distributions.

**Status:** Untested.

### H3 — Diagnosis Validity
> Explanation drift patterns can be used to identify likely drift
> root causes.

**Test:** Inject known drift types, check whether the Drift Diagnosis
Engine correctly classifies them from explanation-drift signals alone.

**Status:** Untested.

**If H1 and H2 validate:** the project has a defensible novel contribution.
**If H1 validates but H2 does not:** EDS is a useful complement to DDS, not a replacement.
**If neither validates:** the diagnosis + recommendation framework (H3) remains the contribution.

---

## 4. Contributions and Evaluation

### Contributions (Methodology)

What we propose and build. These are the novel artifacts of the project.

#### Contribution 1: Explanation Drift Score (EDS)
Quantifies changes in model reasoning over time.

Answers: *"Has the model's reasoning changed?"*

Components under consideration:
- SHAP distribution shift
- Feature importance magnitude shift
- Feature ranking shift
- Jensen-Shannon divergence
- Cosine similarity
- Wasserstein distance

The final formula is a Phase 1 deliverable (see Section 12).

#### Contribution 2: Model Health Score (MHS)
A single interpretable 0–100 health score.

Answers: *"How healthy is the model overall?"*

Inputs:
- Data Drift Score (DDS)
- Explanation Drift Score (EDS)
- Prediction Stability (PS)
- Performance Trend (PT)

Conceptual formula:
MHS = 1 - ( w1·DDS + w2·EDS + w3·(1-PS) + w4·(1-PT) )

Bands:
- 90–100 Healthy
- 70–89  Stable
- 50–69  Warning
- 0–49   Critical

#### Contribution 3: Drift Diagnosis Engine
Identifies root causes of drift with confidence scores.

Answers: *"Why is drift happening?"*

Output format (probabilistic, not categorical):
Covariate Shift: 82%
Class Imbalance: 11%
Concept Drift: 7%

Candidate causes:
- Covariate shift
- Feature drift
- Class imbalance
- Distribution shift
- Concept drift

**Root Cause Confidence Score** is part of this contribution — a
probabilistic distribution over causes, not a single label.

#### Contribution 4: Adaptation Recommendation Engine
Recommends corrective actions, mapped from diagnosed cause.

**Recommendation-only. Not autonomous retraining.**

Examples:
| Diagnosis | Recommendation |
|---|---|
| Covariate Shift | Collect recent data → Retrain model |
| Class Imbalance | Apply SMOTE → Rebalance dataset |
| Feature Drift | Validate feature pipeline |
| Major Distribution Shift | Full model retraining |

### Experimental Evaluation (Evidence)

How we prove the contributions work. This is **not** a contribution itself.

#### Evaluation Metric: Explanation Drift Lead Time (EDLT)
Quantifies how much earlier EDS warns compared to accuracy decline.
EDLT = t(accuracy crosses threshold)

t(EDS crosses threshold)

- Positive EDLT → EDS warned before accuracy dropped
- Zero EDLT → simultaneous signal
- Negative EDLT → accuracy dropped first (hypothesis falsified)

Reported as: **median lead time across N drift scenarios**, with
distribution, plus statistical comparison vs. DDS-only monitoring (for H2).

EDLT is the primary evidence for H1 and H2. It is not itself a contribution.

---

## 5. Metric Uniqueness Rule

**Every metric must answer a different question.**

This rule prevents metric proliferation, which is a common failure mode
in student research projects. Any proposed metric must justify itself
against this table.

| Metric | Question it answers | Type |
|---|---|---|
| DDS | Has the data changed? | Baseline (existing) |
| EDS | Has the model's reasoning changed? | Contribution |
| MHS | How healthy is the model overall? | Contribution |
| DPI | Is health deteriorating faster? | Derived dashboard feature |
| EDLT | How early was the warning? | Evaluation metric |

### Notes on rejected/complementary metrics

- **Explanation Stability Index (ESI)** — Rejected as a separate metric.
  It answers the same question as EDS (has feature importance changed?).
  If needed, ESI is folded into EDS as an internal component, not surfaced
  as a distinct metric.

- **Drift Progression Index (DPI)** — Kept as a derived dashboard feature:
  `DPI = d(MHS)/dt`. It is a rate, not a new measurement. Do not present it
  as a research contribution. Do not spend significant development time on it.

---

## 6. Extended Workflow
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
Drift Progression Index (DPI) ← derived, dashboard-only
↓
Drift Diagnosis Engine
↓
Adaptation Recommendation Engine
↓
Desktop Dashboard
↓
Reports & Analytics
```
**Parallel experimental track (Phases 2–3):**
```
Reference Dataset
↓
Train Model
↓
Drift Simulation Laboratory
↓
Inject controlled drift
↓
Measure DDS, EDS, Accuracy over time
↓
Compute EDLT
↓
Compare vs. DDS-only baseline
↓
Validate H1, H2, H3```

---

## 7. Technology Stack

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

## 8. Datasets

**Primary:** Credit Card Fraud Detection (highly imbalanced, real-world)
**Secondary:** Loan Approval Dataset (easier interpretation, faster prototyping)

**Experiment order:** Start with Loan Approval (Phase 3 initial experiments),
then extend to Credit Card Fraud once pipeline is stable.

---

## 9. Application Modules (Planned — Phase 4)

1. Dashboard — Health score, drift status, alerts, trends
2. Model Monitoring — Model list, performance metrics, health history
3. Drift Monitoring — Data drift, feature drift, concept drift, reports
4. Explanation Drift Analysis — Baseline vs current explanations, feature importance changes, EDS
5. Diagnosis Engine — Root cause analysis with confidence scores
6. Recommendation Center — Suggested actions, recovery plans, mitigation strategies
7. Reports Module — PDF reports, health reports, drift reports
8. Settings — Model configurations, monitoring rules, threshold management

**Drift Simulation Laboratory** — not a user-facing module. This is a
research/experiment module (see Section 11).

---

## 10. Differentiators vs Existing Tools

**Existing:** Evidently AI, Arize AI, Fiddler AI, NannyML
**What they offer:** Drift detection, monitoring, explainability

**Our differentiators:**
- Explanation Drift Score (EDS) as an early warning signal
- Model Health Score (MHS) as a unified health indicator
- Drift Diagnosis Engine with confidence-scored root causes
- Adaptation Recommendation Engine
- Empirical validation via EDLT — proof that EDS warns before accuracy drops

**Critical constraint:**
The project MUST NOT become "Evidently + SHAP + Dashboard".
The desktop application is the **presentation layer** for the research,
not the research itself.

---

## 11. Drift Simulation Laboratory (Research Module)

The experimental backbone of the project. Without this, the project is a
product. With it, the project is research.

**Purpose:**
Controlled environment for injecting drift scenarios and measuring the
resulting signal from DDS, EDS, and accuracy over time.

**Supported drift injections (initial set):**
- Covariate shift
- Concept drift
- Label noise
- Class imbalance
- Feature-specific drift

**Outputs per experiment:**
- Time series of: DDS, EDS, accuracy, MHS
- EDLT computation (per scenario)
- Diagnosis Engine output (for H3 validation)
- Comparison vs. DDS-only baseline (for H2 validation)

**Location:** `experiments/` directory (separate from `backend/`)

**This is built in Phase 2, before any user-facing application code.**

---

## 12. Repository Layout
Explainable-Drift-Detection-Recommendation-System/
├── .github/
│ ├── pull_request_template.md
│ └── ISSUE_TEMPLATE/
│ ├── task.md
│ ├── bug.md
│ └── config.yml
├── experiments/ ← Drift Simulation Lab, EDS/EDLT experiments
├── backend/ ← FastAPI backend (Phase 4)
├── docs/ ← Documentation, diagrams, papers
├── ExplainAI/ ← Tauri + React desktop app (Phase 4)
│ ├── src/
│ ├── src-tauri/
│ └── ...
├── CONTEXT.md ← this file
├── .gitignore
├── .gitattributes
├── LICENSE
└── README.md


---

## 13. Four-Phase Roadmap

The single most important structural decision in this project.
The research value is created in Phases 1–3.
The application is Phase 4.

**Do not skip ahead. Phase 4 first is the common failure mode.**

### Phase 1 — Research Definition

Deliverables:
- [ ] Finalize EDS formula (components, weights, normalization)
- [ ] Finalize hypotheses H1, H2, H3
- [ ] Design EDLT measurement protocol (thresholds, baselines)
- [ ] Design Drift Simulation Laboratory (scenarios, injection methods, outputs)
- [ ] Formalize MHS formula and weight-selection strategy
- [ ] Identify evaluation metrics beyond EDLT (if needed)

Exit criteria: All formulas and experiment protocols are written down and
reviewable. Nothing is code yet.

### Phase 2 — Experimental Infrastructure

Deliverables:
- [ ] `experiments/` scaffold in Python
- [ ] Drift injection engine
- [ ] Dataset pipeline (Loan Approval first)
- [ ] SHAP pipeline
- [ ] EDS, DDS, MHS computation modules
- [ ] EDLT computation
- [ ] Result storage (CSV/Parquet is fine for Phase 2)

Exit criteria: Can run one full drift scenario end-to-end and produce a
time-series table of DDS, EDS, accuracy, EDLT.

### Phase 3 — Validation

Deliverables:
- [ ] Run H1 experiments across ≥4 drift scenarios
- [ ] Run H2 comparison (EDS vs. DDS-only)
- [ ] Run H3 experiments (diagnosis correctness)
- [ ] Statistical analysis of EDLT distributions
- [ ] Experimental write-up (draft paper section)

Exit criteria: Documented answer to H1, H2, H3 — validated or falsified.
At least one plot showing EDS preceding accuracy decline.

### Phase 4 — Productization

Deliverables:
- [ ] FastAPI backend exposing experiment results + live monitoring
- [ ] SQLite → schema design
- [ ] Tauri + React dashboard (all 8 modules)
- [ ] Reports module
- [ ] Settings module
- [ ] PyInstaller sidecar packaging
- [ ] Installer build

Exit criteria: A single-installer desktop application presenting the
research, usable by an evaluator without setup.

---

## 14. Repository Setup Status

| Item | Status |
|---|---|
| GitHub repo created | ✅ |
| Repo visibility | Private (will go public later) |
| Collaborators added | ❌ Not yet |
| Branch protection rule | ✅ Configured (activates when public) |
| Root `.gitignore` | ✅ |
| Tauri scaffold committed | ✅ |
| GitHub templates committed | ✅ |
| CONTEXT.md committed | ✅ |
| Phase 1 deliverables | ❌ Not started |
| Backend skeleton (Phase 4) | ❌ Not started — intentionally delayed |

---

## 15. Development Workflow

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
- Member 1: Phase 2 experimental infrastructure + Phase 4 frontend
- Member 2: Phase 1 formalization + Phase 2 SHAP/drift pipeline
- Member 3: Phase 3 evaluation design + Phase 4 backend + documentation

Adjust once Phase 1 is complete and skill overlap is clearer.

---

## 16. Long-Term Research Goals

Potential future extensions (beyond scope of current project):
1. Semi-automatic adaptation
2. Drift prediction
3. Regression support
4. NLP drift detection
5. Image model drift detection
6. Real-time streaming data
7. Self-healing ML systems

---

## 17. Current Next Steps

**We are in Phase 1.**

1. ✅ Repo setup + templates + CONTEXT.md
2. ⏳ Formalize EDS formula
3. ⏳ Formalize MHS formula
4. ⏳ Design EDLT protocol
5. ⏳ Design Drift Simulation Laboratory
6. ⏳ Get Phase 1 reviewed by project coordinator
7. ⏳ Move to Phase 2

**Explicitly deferred:** backend skeleton, Tailwind/shadcn setup, UI pages.
These are Phase 4 concerns.

---

**Last Updated:** 28 Sep 2026
**Maintained By:** Soham (repo owner)