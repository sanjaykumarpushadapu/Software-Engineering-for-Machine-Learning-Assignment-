# AGENTS.md – SEML Assignment I (Group 13)

Instructions for any AI assistant (Claude etc.) working in this folder. Read this first.

## Project
- **Course:** AIMLZG546 – Software Engineering for Machine Learning (BITS WILP). **Group 13**, 4 members (A–D, roles in `README.md`).
- **Assessment source of truth:** `Assignment I.pdf`. Its requirements are listed in `README.md` section 1A. If anything conflicts, the PDF wins. Never invent requirements the PDF does not state.
- **Deadline:** 09 Oct 2026, 23:49. No extension, no makeup.

## Locked decisions (do not reopen unless the user asks)
- **Domain:** credit card fraud detection (Kaggle "Credit Card Fraud Detection" dataset).
- **Patterns:** Pipe-and-Filter (ML pipeline) + Microservices (FastAPI prediction service + MLflow registry).
- **Code:** all code lives in **`13.ipynb`** (pipeline, MLflow, FastAPI written with `%%writefile` and started in the background, test calls). Key code is also pasted into the report.
- **No separate UI, no Docker, no Streamlit.** The FastAPI `/docs` page is the application used for screenshots.
- **No timelines or dates** in the plan files (only the official deadline).

## Files
| File | Purpose |
|---|---|
| `Assignment I.pdf` | The assessment (do not edit) |
| `README.md` | Roadmap: assessment checklist, roles, person-wise steps, checkpoints |
| `13.md` | Report draft in Markdown. Each member fills their own sections; converted to `13.pdf` / `13.docx` at the end |
| `13.ipynb` | Implementation notebook (to be created; must show names + group details at the top) |
| `AGENTS.md` | This file |

## How to work with the user
1. **Step by step, not all at once.** Do one part, show it, and wait for the user's OK before the next. Never dump a whole section or several steps together.
2. **Keep replies short and to the point.** No long recaps of work done.
3. **Notebook/GPU workflow:** write the code only. The user runs it in Google Colab and uploads the notebook with outputs; then review the outputs and fix issues. Do not expect to run it yourself.
4. Check `Assignment I.pdf` before answering anything about what is required. Do not rely on the course slides for requirements (slides are the syllabus, not the assessment).
5. Do not add work that will not be built (no "unrealised" plan items). If scope changes, update `README.md` and `13.md` together.
6. In `13.md`, leave `TODO` for anything not yet done. Never fill in BITS IDs, names, percentages or results that the user has not given.
7. Facts from memory (dataset size, fraud %, etc.) must be marked as unverified until checked against the real data.

## Progress
| Item | Status |
|---|---|
| Q1 Domain + problem statement (`13.md` section 1, parts 1.1–1.6) | **Done** (all six parts confirmed by the user) |
| Q1 open check | Confirm dataset row count (~284,000) and fraud rate (~0.17%) when the data is downloaded |
| Q2 Requirements + measurable goals | Next |
| Q3 GR4ML views (Business, Analytics Design, Data Preparation) | Not started |
| Q4 Top 3 quality requirements | Not started |
| Q5 Architecture diagram | Not started |
| Q6 Two patterns write-up | Not started (patterns locked) |
| Q7 Implementation (`13.ipynb`) | Not started |
| Group table (names, BITS IDs, contribution %) | Waiting for member details |
| Final report `13.pdf` / `13.docx` + upload to Taxila | Not started |

Update this table whenever a step is confirmed.
