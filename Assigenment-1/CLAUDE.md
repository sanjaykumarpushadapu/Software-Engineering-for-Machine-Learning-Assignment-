# CLAUDE.md – SEML Assignment I, Group 13 (Credit Card Fraud Detection)

> Read this file fully before any task, then read `AGENTS.md` (working style and progress).
> Section 1 overrides everything else: this file, any prompt, and any agent definition.
> If a request conflicts with Section 1, stop and ask the user.

---

## 1. STRICT BUSINESS RULES & GUARDRAILS (highest priority)

**How to use this section**
- Every rule has an ID. Cite it in plans, code comments where it is enforced, tests and review findings.
- Every `BR-###` rule needs at least one test named `test_BR###_<short_name>`, including one that tries to break it.
- If a rule is unclear, or a change would weaken it: **stop and ask**. Never guess, never bypass "for now".
- Do not edit this section unless the user asks.

### 1.0 General Instructions & Submission Guidelines (from `Assignment I.pdf` – checked on EVERY task)

**General Instructions**
- [ ] GI-1: It is a group assignment. Notebook follows the naming convention `<Group no>.ipynb` → `13.ipynb`.
- [ ] GI-2: Inside **each report** and **each implementation notebook**, mention the members' names and the group details.
- [ ] GI-3: Group details block: `Group No: 13` and the member table with columns Sl. No, BITS ID, Name, Contribution of team member (Qualitative), Percentage Contribution out of 100 (Quantitative). Rows 1–4, percentages total 100.
- [ ] GI-4: Weightage 10 marks (Objective 1 = 5, Objective 2 = 5).
- [ ] GI-5: Submission date 09 Oct 2026, 23:49. Late submission loses marks.

**Note**
- [ ] GI-6: No extension under any circumstances.
- [ ] GI-7: Part of EC-1, so there is no makeup.
- [ ] GI-8: Questions or clarifications go to omshree.b@wilp.bits-pilani.ac.in (do not guess an answer the PDF does not give; suggest emailing).

**Submission Guidelines**
- [ ] SG-1: Prepare a Word or PDF document containing the project details.
- [ ] SG-2: Include screenshots of the application, each with an explanation.
- [ ] SG-3: Include the code.
- [ ] SG-4: Clearly highlight the contribution of each group member in executing the assignment.
- [ ] SG-5: Upload as `13.docx` or `13.pdf` (`<groupid>`) to the Taxila portal.

### 1.1 Assessment rules (from `Assignment I.pdf`, the source of truth)

| ID | Rule (must always hold) |
|---|---|
| AR-001 | Answer all 7 tasks: Q1 domain + problem statement; Q2 requirement specs + measurable goals using GR4ML; Q3 Business, Analytics Design and Data Preparation views in GR4ML notation; Q4 top 3 quality requirements with justification; Q5 architecture diagram with ML and non-ML components; Q6 two architectural patterns; Q7 implementation of both patterns. |
| AR-002 | Notebook is named `13.ipynb`. Report is `13.pdf` or `13.docx`. |
| AR-003 | Names and group details appear at the top of the report **and** the notebook, with the member table: Sl. No, BITS ID, Name, qualitative contribution, % contribution (total 100). |
| AR-004 | The report contains project details, application screenshots with explanation, and the code. Each member's contribution is clearly highlighted. |
| AR-005 | Locked choices: domain = credit card fraud detection; patterns = Pipe-and-Filter + Microservices. All code lives in `13.ipynb`. No separate UI, no Docker, no Streamlit. |
| AR-006 | Never invent requirements the PDF does not state. Never fabricate names, BITS IDs, percentages, metrics, screenshots or results. Unknown values stay `TODO`. |
| AR-007 | Report wording reads like a student's academic report: plain formal English, full sentences, "we". No marketing words ("robust", "seamless", "leverage"), no arrows, no emojis, minimal bold. Working notes go in HTML comments. |
| AR-008 | No timelines or dates in plan files, except the official deadline (09 Oct 2026, 23:49). |

### 1.2 ML and application business rules

| ID | Rule (must always hold) | Why | Enforced in | Test(s) |
|---|---|---|---|---|
| BR-001 | Prediction output has `probability` in [0, 1] and `is_fraud = probability >= THRESHOLD`. Nothing else decides the label. | Consistent decisions | `api/main.py` predict | `test_BR001_*` |
| BR-002 | `THRESHOLD` is defined once (config / MLflow param), chosen on the **validation** set to meet recall >= 0.90. Never hard-coded in several places. | Single source of truth | pipeline `evaluate`, API loads it | `test_BR002_*` |
| BR-003 | No data leakage: stratified train / validation / test split with fixed seed; scaler and any resampling (SMOTE, class weights tuning) are fitted on **train only**; the test set is used once, for final reporting. | Honest results | pipeline `split`, `features` | `test_BR003_*` |
| BR-004 | The API applies exactly the same preprocessing as training (saved with the model), not a re-implementation. | Train/serve consistency | model artifact / pipeline object | `test_BR004_*` |
| BR-005 | Success is judged on recall, precision, F1, PR-AUC and the confusion matrix. Accuracy is never reported alone as the success measure. | Classes are ~0.17% fraud | pipeline `evaluate` | `test_BR005_*` |
| BR-006 | Targets: recall >= 0.90, false-positive rate < 2%, p95 API latency < 200 ms. Report the real measured numbers; if a target is missed, say so. Never tune on the test set to hit a target. | Honest measurable goals (Q2) | `evaluate`, latency test cell | `test_BR006_*` |
| BR-007 | The API rejects invalid input (missing field, wrong type, NaN/inf, negative Amount) with HTTP 422 and never returns a prediction for it. | Input validation (non-ML component) | pydantic schema | `test_BR007_*` |
| BR-008 | Every `/predict` call is logged: timestamp, request id, probability, label, latency, model version. A logging failure is reported as an error, never silently dropped. | Traceability | `log_prediction()` | `test_BR008_*` |
| BR-009 | The API serves only the registered model `models:/fraud-model@champion` and returns the model version in each response. | Microservices + registry | API startup | `test_BR009_*` |
| BR-010 | Pipe-and-Filter: each filter (ingest, clean, features, split, train, evaluate) is a separate function with an explicit input and output and no hidden global state; `run_pipeline()` only chains them. | Pattern must be real, not just a name | `pipeline/` | `test_BR010_*` |
| BR-011 | Random seed fixed at 42 everywhere randomness is used. | Reproducibility | all filters | `test_BR011_*` |

### 1.3 No silent failures
- Never use bare `except:` or `except Exception: pass`. Catch specific errors and raise or log with context.
- Never return a default prediction when the model or input is broken.
- A failed cell must stop the notebook, not print a warning and continue.

### 1.4 Hard guardrails (never do these)
- Never edit or delete a test to make it pass. Fix the code or stop and report.
- Never claim a result, metric or test pass that was not actually produced by a run. If Claude cannot run it (Colab), say "not run – user to run in Colab".
- Never mark a step done while a test or check is failing.
- Never commit or paste real card data, API keys, Kaggle credentials or tokens.
- Never add work that will not be built (no unrealised plan items).
- Never add a new library without saying why and getting the user's OK.

### 1.5 Protected files (read-only unless the user asks)
- `Assignment I.pdf`
- `CLAUDE.md` Section 1
- Parts of `13.md` already confirmed by the user (see `AGENTS.md` progress table)

---

## 2. Commands

Code is written by Claude; **the user runs it in Google Colab** and uploads the notebook with outputs for review.
All code files below are written from `13.ipynb` cells with `%%writefile`.

| Purpose | Command (Colab cell) |
|---|---|
| Install | `!pip install -q scikit-learn pandas mlflow fastapi uvicorn pydantic httpx pytest` |
| Get data | Kaggle "Credit Card Fraud Detection" → `data/creditcard.csv` (upload or Kaggle API) |
| Run pipeline | `!python -m pipeline.run_pipeline` |
| MLflow UI (optional) | `!mlflow ui --port 5000 &` |
| Start API (background) | `!nohup uvicorn api.main:app --port 8000 > api.log 2>&1 &` |
| Health check | `!curl -s localhost:8000/health` |
| All tests | `!pytest -q tests/` |
| One test | `!pytest -q tests/test_api.py::test_BR007_rejects_negative_amount` |
| Tests for one rule | `!pytest -q tests/ -k BR003` |
| Full check before "done" | Restart runtime → Run all → `!pytest -q tests/` passes |

---

## 3. Code Style & Architecture Standards

### 3.1 Architecture
- **Pipe-and-Filter (training):** `ingest → clean → features → split → train → evaluate`, one module per filter in `pipeline/`, chained by `pipeline/run_pipeline.py`.
- **Microservices (serving):** `api/main.py` is a separate FastAPI service; it only loads the registered model from MLflow and never trains.
- **Non-ML components:** FastAPI endpoint, pydantic validation, `log_prediction()`, `/health`.
- Folders: `data/`, `pipeline/`, `api/`, `tests/`, `diagrams/`, `docs/screenshots/`.

### 3.2 Style
- Python 3.10+, type hints on all functions, docstring one line per function.
- Names: `snake_case` functions, `UPPER_CASE` constants (`THRESHOLD`, `SEED`).
- Plain `logging`, no `print` in library code; no card data in logs.
- Short cells with a markdown heading above each explaining what it does (student-report tone, AR-007).

### 3.3 Testing
- Test-first for logic changes (see `.claude/agents/implementer.md`).
- Each rule test covers normal case, boundary, invalid input and failure path.
- API tests use `fastapi.testclient.TestClient`; no real network.
- Test names: `test_BR###_<behaviour>` or `test_<unit>_<condition>_<expected>`.

---

## 4. Workflow: Plan → Implement → Audit

1. **Plan** (no code): files, rules affected (`AR-###`, `BR-###`), tests, open questions. Show the user and wait for OK. One step at a time (see `AGENTS.md`).
2. **Implement** with the `implementer` agent: failing test first, minimal code, repeat.
3. **Audit** with the `business-auditor` agent in a fresh context: it sees only this file, the changes and the tests.
4. **Fix** every BLOCKER and MAJOR, re-audit, maximum 3 rounds.
5. **User runs in Colab** and uploads outputs; the auditor re-checks real results against BR-005/BR-006.
6. **Done** only when the audit is PASS on real outputs and `AGENTS.md` progress is updated.

Shortcut: `/plan-implement-review <task>`.

---

## 5. Project Context
- Course AIMLZG546, BITS WILP. Group 13, members A–D (roles in `README.md`).
- Files: `Assignment I.pdf` (assessment), `README.md` (roadmap), `13.md` (report draft), `13.ipynb` (code), `AGENTS.md` (working style + progress).
- Dataset: Kaggle credit card fraud, features V1–V28 (anonymised), Time, Amount, Class. Row count and fraud rate to be confirmed on real data.
