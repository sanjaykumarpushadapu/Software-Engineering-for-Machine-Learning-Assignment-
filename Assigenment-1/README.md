# SEML Assignment 1 – Team Roadmap

**Course:** AIMLZG546 – Software Engineering for Machine Learning
**Group No:** 13  **Deadline:** 09 Oct 2026, 23:49 (no extension, no makeup – EC-1)  **Marks:** 10
**Report draft:** [`13.md`](13.md) – every member fills their own sections; convert to PDF/DOCX at the end.
**Live doc:** https://claude.ai/code/artifact/8bbe635c-3990-4ad2-ae54-a4c2a8e047b1

---

## 1. Assignment at a glance

| Part | Task | Marks |
|---|---|---|
| Objective 1 – Requirements | 1. Domain + problem statement | 5 |
| | 2. Requirement specs + measurable goals (GR4ML) | |
| | 3. GR4ML views: Business, Analytics Design, Data Preparation | |
| | 4. Top 3 quality requirements + justification | |
| Objective 2 – Architecture | 5. Architecture diagram (ML + non-ML components) | 5 |
| | 6. Select 2 architectural patterns | |
| | 7. Implement both patterns (working app) | |

**Deliverables**
- `13.ipynb` – implementation notebook with group details
- `13.pdf` / `13.docx` – report: project details, app screenshots + explanation, code, contribution table (BITS ID, name, qualitative contribution, % out of 100)
- Upload to Taxila. Queries: omshree.b@wilp.bits-pilani.ac.in

**Locked project:** Credit card fraud detection (Kaggle dataset).
**Locked patterns:** Pipe-and-Filter (ML pipeline) + Microservices (FastAPI prediction service, MLflow registry). Everything runs from one notebook (`13.ipynb`) in Colab or a laptop, no GPU. The FastAPI `/docs` page is used for the application screenshots.

## 1A. Assessment instructions and guidelines (from the PDF – do not miss)

**General instructions**
- [ ] Group assignment; follow the naming convention `<Group no>.ipynb` → **`13.ipynb`**
- [ ] Inside **each report and each implementation notebook**, mention **name and group details**
- [ ] Group details block at the top: Group No (13) and the member table – Sl. No, BITS ID, Name, Contribution (qualitative), Percentage contribution (out of 100, quantitative)

**Marks and deadline**
- [ ] Weightage: 10 marks (Objective 1 = 5, Objective 2 = 5)
- [ ] Submission date: **09 Oct 2026, 23:49**. Late submissions lose marks
- [ ] No extension under any circumstances; part of EC-1 so there is **no makeup**
- [ ] Questions or clarifications: omshree.b@wilp.bits-pilani.ac.in

**Project details (what must be in the work)**
- [ ] 1. Domain + problem statement for an ML-based application
- [ ] 2. Requirement specifications + measurable goals using GR4ML concepts
- [ ] 3. GR4ML views: Business View, Analytics Design View, Data Preparation View
- [ ] 4. Top three quality requirements with justification for the selection
- [ ] 5. System architecture diagram showing both ML and non-ML components (any tool)
- [ ] 6. Select and apply any two relevant architectural patterns
- [ ] 7. Implement the selected patterns with appropriate technologies and tools

**Submission guidelines**
- [ ] One Word/PDF document containing the project details
- [ ] **Screenshots of the application, with explanation**
- [ ] The **code**
- [ ] **Clearly highlight the contribution of each group member**
- [ ] Upload as `13.docx` or `13.pdf` (`<groupid>`) to the **Taxila portal**

## 2. Team roles

| Member | BITS ID | Role | Owns | Main outputs |
|---|---|---|---|---|
| A | | Lead / Business analyst | Obj 1: Q1, Q2 + Business View; final report | Problem statement, specs, measurable goals, Business View, report, prediction logging |
| B | | GR4ML modeller / Integrator | Obj 1: Q3 (Analytics Design + Data Prep views) | 2 GR4ML diagrams, end-to-end run of the notebook, test calls, screenshots |
| C | | System architect | Obj 1: Q4; Obj 2: Q5, Q6 | Top 3 quality reqs, architecture diagram, pattern write-ups, FastAPI service |
| D | | ML engineer | Obj 2: Q7 (ML side) | EDA, Pipe-and-Filter pipeline, MLflow registry, `13.ipynb` |

## 3. Person-wise work plan (detailed)

Each step shows **what to do**, the **output** (file to produce) and **done when** (how you know it's finished).
Repo folders used below: `docs/`, `diagrams/`, `notebook/`. The notebook writes `pipeline/` and `api/` code files itself (via `%%writefile`).

---

### Member A – Lead / Business analyst

**A1. Finalise domain + problem statement**
- Run a 30-min team call; confirm domain (credit card fraud detection) and the 2 patterns.
- Write 5–6 lines: who has the problem (bank), what goes wrong (fraud losses, slow manual review), what ML does (flags risky transactions in real time), expected benefit.
- Output: `docs/01_problem_statement.md`
- Done when: all 4 members agree in the group chat.

**A2. Project setup**
- Create GitHub repo `SEML-Group13` with the folders above; add all members.
- Create shared report doc with headings Q1–Q7 + the contribution table from the PDF.
- Done when: everyone can push code and edit the report.

**A3. Requirement specs + measurable goals**
- Functional requirements, e.g. FR1 accept a transaction via API · FR2 return fraud label + probability · FR3 show result on the FastAPI `/docs` page · FR4 retrain monthly · FR5 log every prediction.
- Measurable goals table (Goal → Metric → Target): catch fraud → recall ≥ 90% · don't annoy customers → false-positive rate < 2% · decide during payment → p95 latency < 200 ms · cut losses → fraud loss down 30%.
- Output: `docs/02_requirements.md`
- Done when: shared with C (needed for quality requirements).

**A4. GR4ML Business View**
- In draw.io using B's GR4ML legend, draw:
  - Actors: bank risk manager, fraud analyst, customer
  - Strategic goal: reduce fraud losses
  - Indicators: fraud loss %, false-alarm rate
  - Decision goal: allow / block / send to review
  - Question goal: is this transaction fraudulent?
  - Insight: fraud probability score
- Output: `diagrams/business_view.png` + 1 paragraph explanation.
- Done when: every element links to a goal from A3.

**A5. Prediction logging (non-ML component)**
- Write a small `log_prediction()` function (in the notebook, cell shared with C) that appends each request, prediction, probability and latency to `predictions_log.csv`.
- C's API calls it on every `/predict`.
- Done when: after test calls, the log file has one row per call; screenshot sent to B.

**A6. Compile report**
- Paste sections in order Q1–Q7, add diagrams, screenshots, code (appendix).
- Each member writes 2 lines on their work; agree % together (total 100).
- Output: `13.pdf`
- Done when: every item in the submission checklist (section 6) is ticked.

**A7. Submit**
- Final read-through with team → upload to Taxila → post confirmation screenshot in group.

---

### Member B – GR4ML modeller / Integrator

**B1. Learn GR4ML + legend**
- Go through Session 3 slides 13–24 (credit-risk toy example).
- In draw.io make a small shape set for each GR4ML element so A and B diagrams look the same.
- Output: `diagrams/gr4ml_legend.png`
- Done when: shared with A.

**B2. GR4ML Analytics Design View**
- Analytics goals: prediction goal "predict fraud per transaction"; description goal "understand fraud patterns".
- Algorithms (alternatives): Logistic Regression, Random Forest, XGBoost (use D's baseline results).
- Softgoals: high recall, low latency, interpretability.
- Influence links (+/–): e.g. XGBoost ++ recall, – interpretability; Logistic Regression ++ interpretability, – recall.
- Output: `diagrams/analytics_design_view.png` + explanation.

**B3. GR4ML Data Preparation View**
- Get from D: columns, cleaning steps, features.
- Entities: raw transactions → cleaned transactions → training set / test set.
- Preparation tasks + operators: remove duplicates, scale Amount/Time (StandardScaler), handle imbalance (class weights/SMOTE), stratified 80/20 split.
- Draw data-flow arrows between entities through the tasks.
- Output: `diagrams/data_preparation_view.png` + explanation.

**B4. End-to-end run + test calls**
- In a fresh Colab runtime, run `13.ipynb` top to bottom (pipeline → MLflow → start API → test calls).
- Send one known fraud row and one genuine row to `/predict` and check the results and the log file.
- Log any bug in the group chat and tag the owner.
- Done when: the notebook runs from top to bottom with no manual fixes.

**B5. Screenshots**
- Capture: pipeline console run, MLflow runs + registered model, FastAPI `/docs` page, test call with a fraud row, test call with a genuine row, prediction log file.
- Write 2–3 lines under each.
- Output: `docs/screenshots/` → send to A.

**B6. Final review**
- Check the 3 GR4ML views are consistent with each other and with A's goals.

---

### Member C – System architect

**C1. Shortlist patterns**
- Read Session 4 (pipe-and-filter), Session 5 (CQRS, microservices), Session 6 (event-driven).
- Propose 2 patterns with a 3-line reason each; confirm at kick-off.

**C2. Top 3 quality requirements**
- For each: definition, measurable target, why it matters for fraud, how the architecture supports it.
  1. Accuracy (recall ≥ 90%) – a missed fraud is direct money loss.
  2. Performance (p95 < 200 ms) – decision must happen during payment.
  3. Security & privacy – card data is sensitive; API auth, no raw card numbers stored.
- Output: `docs/03_quality_requirements.md`

**C3. System architecture diagram**
- draw.io, with a colour legend for ML vs non-ML:
  - Non-ML: FastAPI endpoint, request validation, prediction log, `/health` check, notebook runner.
  - ML: training pipeline (ingest → clean → features → train → evaluate), MLflow model registry, model inference, retraining trigger.
- Show two flows: request flow (client → API → model → response → log) and training flow (data → pipeline → registry).
- Output: `diagrams/architecture.png`

**C4. Pattern write-up**
- For each pattern: problem/context → solution → how we applied it → small diagram → benefit and trade-off.
- Output: `docs/04_patterns.md`

**C5. FastAPI prediction microservice**
- `api/main.py`: load model from MLflow at startup (`models:/fraud-model@champion`); `POST /predict` (pydantic input) → `{is_fraud, probability}`; `GET /health`.
- Log latency per request; fire 100 test requests and note p95.
- Done when: endpoint + sample JSON sent to A.

**C6. Report section**
- Q5–Q7 text + diagrams + p95 result → send to A.

---

### Member D – ML engineer

**D1. Data + EDA**
- Download Kaggle "Credit Card Fraud Detection" (`creditcard.csv`).
- In Colab: shape, nulls, class balance (fraud is < 1%), Amount/Time distributions.
- Output: `notebook/01_eda.ipynb` + 5-line summary to team.

**D2. Baseline model**
- Stratified 80/20 split; scale Amount/Time; train Logistic Regression + Random Forest (`class_weight='balanced'`).
- Report recall, precision, F1, PR-AUC.
- Send B the cleaning steps + feature list; send results to B for the Analytics Design View.

**D3. Pipe-and-Filter pipeline**
- One module per filter, same interface (DataFrame in → DataFrame/model out): `ingest.py`, `clean.py`, `features.py`, `train.py`, `evaluate.py`; `run_pipeline.py` chains them.
- Show the pattern: swap one filter (e.g. model) without touching the others.
- Done when: `python run_pipeline.py` runs end to end and prints each stage.

**D4. MLflow model registry**
- Log params, metrics and model inside train/evaluate; register as `fraud-model`; set alias `champion` on the best version.
- Done when: C can load `models:/fraud-model@champion` (tell C).

**D5. Final notebook**
- `13.ipynb`: names + group details table on top, EDA, pipeline run, metrics, confusion matrix, how to run the API and test calls.

**D6. Final review**
- Re-run notebook top to bottom; check outputs are saved.

---

### Hand-offs between members
| From → To | What |
|---|---|
| B → A | GR4ML legend |
| D → B | Features, cleaning steps, baseline results |
| A → C | Measurable goals |
| D → C | Model registered in MLflow |
| A → C | `log_prediction()` function |
| All → A | Sections + screenshots for report |

## 4. Step-by-step plan

| # | Phase | Step | Owner | Status |
|---|---|---|---|---|
| 1 | 0 Kick-off | Finalise domain + problem statement (all agree) | A | ☐ |
| 2 | 0 Kick-off | Shared doc, GitHub repo, report template with contribution table | A | ☐ |
| 3 | 0 Kick-off | Study GR4ML notation (Session 3); set up draw.io | B | ☐ |
| 4 | 0 Kick-off | Shortlist 2 architectural patterns (Sessions 4–6) | C | ☐ |
| 5 | 0 Kick-off | Download dataset; first EDA | D | ☐ |
| 6 | 1 Requirements | Requirement specs + measurable goals (e.g. recall ≥ 90%, latency < 200 ms, false positives < 2%) | A | ☐ |
| 7 | 1 Requirements | Business View: actors, strategic/decision/question goals, insights, indicators | A | ☐ |
| 8 | 1 Requirements | Analytics Design View: analytics goals, algorithms, softgoals | B | ☐ |
| 9 | 1 Requirements | Data Preparation View: entities, prep tasks, operators, data flows (inputs from D) | B | ☐ |
| 10 | 1 Requirements | Top 3 quality requirements + justification | C | ☐ |
| 11 | 1 Requirements | Baseline model; share features + cleaning steps with B | D | ☐ |
| 12 | 2 Build | System architecture diagram (ML + non-ML) | C | ☐ |
| 13 | 2 Build | Pattern write-up: why Pipe-and-Filter + Microservices, one diagram each | C | ☐ |
| 14 | 2 Build | Pipe-and-Filter pipeline: ingest → clean → features → train → evaluate | D | ☐ |
| 15 | 2 Build | Log trained model to MLflow registry | D | ☐ |
| 16 | 2 Build | FastAPI prediction microservice loading model from MLflow | C | ☐ |
| 17 | 2 Build | Prediction logging (non-ML) used by the API | A | ☐ |
| 18 | 2 Build | End-to-end notebook run + test calls | B | ☐ |
| 19 | 3 Report | App screenshots with explanations | B | ☐ |
| 20 | 3 Report | Clean `13.ipynb`: code, metrics, group details | D | ☐ |
| 21 | 3 Report | Architecture + patterns section of report | C | ☐ |
| 22 | 3 Report | Compile report; fill contribution table | A | ☐ |
| 23 | 4 Submit | Everyone reviews; upload `13.pdf` to Taxila | All | ☐ |

## 5. Checkpoints

| Checkpoint | Must be true |
|---|---|
| Domain locked | Problem statement, dataset and 2 patterns agreed |
| Objective 1 done | Specs, goals, 3 GR4ML views, top 3 quality requirements drafted |
| App working | Pipeline → MLflow → FastAPI → test calls run end to end in the notebook |
| Report draft | All sections in, screenshots added, contribution % sums to 100 |

## 6. Submission checklist

- [ ] Group no, names, BITS IDs in both report and notebook
- [ ] Contribution table: qualitative + % per member, total 100
- [ ] Three GR4ML views in GR4ML notation, each tied to the goals
- [ ] Each quality requirement measurable and linked to an architecture choice
- [ ] Architecture diagram labels ML vs non-ML components
- [ ] Both patterns explained and shown working in screenshots
- [ ] Code included; files named `13.ipynb` and `13.pdf`
- [ ] Uploaded to Taxila before the deadline
