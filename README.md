# PwC-Challenge
# StyleVerse AI Control Tower

**Finalist, PwC Challenge 8th Edition** · Team InkSync, IIM Bodh Gaya

An AI-first prototype that cuts a fashion retailer's trend-to-store cycle from **14 weeks to under 8** while keeping every creative and commercial decision with a person.


---

## What's in this repository

| File | What it is |
| --- | --- |
| `<prototype file name>.html` | The full interactive prototype in a single file. Open it in any browser; no installation or internet connection needed. |
| `<case file name>.pdf` | The PwC Challenge case brief(s) the solution responds to. |

## How to view the prototype

1. Download the `.html` file (or clone this repository).
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. Use the left menu to move between pages and the left panel to change settings. Every number recalculates live.

**Suggested walkthrough (5 minutes):**
1. **Trend to design journey:** the problem, the five stages, and the 14 → under 8 weeks result.
2. **AI Design Copilot:** accept, modify or reject each AI suggestion, then pick a short first cut.
3. **Concept room:** approve all four teams and sign the record.
4. **Refinement & trail:** generate the tech pack and see your own decisions in the audit trail.
5. **Executive overview:** open *Scenario assumptions*, pick *Festive surge*, and watch the risks and recommendations change.
6. **Decision review:** accept or reject recommendations and see them logged.

## The problem

StyleVerse, the retailer in the PwC case, has 5 brands and ₹5,600 Cr in revenue. It took 14 weeks to turn a trend into a product in stores:

- Designers spent only **37%** of their week on design. The rest went on manual research, paperwork and approval meetings held one after another.
- Nearly **one-third** of approved designs lost momentum before launch.
- **₹380 Cr** a year was lost to markdowns, and 26% of inventory went unsold.

The goal was to reach **under 8 weeks** and **60%+ creative time**, with **no layoffs** and **provable human authorship** of designs.

## The solution

One principle runs through the whole product: **AI proposes, a person decides, and every decision is recorded.**

### Design workflow (Trend-to-Design Accelerator)

| Module | What it does | Stage impact (modelled) |
| --- | --- | --- |
| **Trend Intelligence** | Scans six signal sources and ranks rising trends. Writes a design brief with a source for every line. | Research: 3 weeks → ~1 |
| **AI Design Copilot** | Drafts design directions. The designer **accepts, modifies or rejects** each element and sets the first-batch size. | Concept: 2 weeks → ~1.3 |
| **Review Accelerator** | Design, Merchandising, Marketing and Commercial review one shared record at the same time. Auto-approval only inside pre-agreed limits. | Review: 3 weeks → ~1.2 |
| **Generated documentation** | Tech pack, spec sheet and bill of materials are written from the signed record. | Refinement: 2 weeks → ~5 days |

Each brand gets its own level of AI. Fast fashion gets full agentic support; luxury gets research support only, because craftsmanship is the product.

### Retail Control Tower (downstream impact)

A faster design cycle means stores don't have to bet the whole season upfront. The control tower:

- Forecasts demand for every product in every store.
- Classifies each position (stockout risk, imbalanced, breakout, slow mover, supply blocked).
- Ranks recommended actions by value: **replenish, transfer, recut, markdown or escalate**.
- Recalculates live under **what-if scenarios** (festive surge, supplier failure, warm winter, cash-tight quarter).
- Lets a planner **accept or reject** each recommendation. Every decision is saved to a decision log.

## Architecture

```
Data sources (runway, social, search, competitors, reviews, sales, weather, Google Trends)
        │
        ├── Design workflow ──── Trend Intelligence → Design Copilot → Review → Generated docs
        │
        └── Control Tower ────── Supabase (PostgreSQL) → XGBoost model → FastAPI → Dashboard
                                                                                   │
                                Audit trail: concept record + planner decision log ┘
```

## Tech stack

This repository contains the frontend prototype, which runs entirely in the browser with built-in demo data. The full build behind it also included the backend, database and model listed below.

| Layer | Technology |
| --- | --- |
| Frontend | HTML, CSS, vanilla JavaScript (single file), deployed on Vercel |
| Backend | FastAPI + Uvicorn (Python 3.11), deployed on Render |
| Database | Supabase (PostgreSQL): 27 tables, ~640,000 rows |
| Machine learning | XGBoost, scikit-learn, LightGBM (benchmarking), pandas |
| External data | Open-Meteo (weather), Google Trends via pytrends |
| AI-assisted development | Claude (Anthropic) |

## The demand model

- **Model:** XGBoost regression (`DEMAND_V1`), 28 features, including 28-day rolling sales, sub-category, price, festival week, return rate, supplier lead time, weather and trend score.
- **Accuracy metric:** `100% × (1 − WAPE)`, measured on non-zero demand weeks, with a chronological train/test split.

| Model | Data | Accuracy |
| --- | --- | --- |
| DEMAND_V1 (fashion) | Synthetic StyleVerse data (250 SKUs × 10 stores × 26 weeks) | 47.7% |
| Same method | 860,871 rows of real retail data | **70.2%** |
| Business baseline (case) | n/a | 62.0% |

Fashion demand is sparse: a single product often sells 0 to 2 units a week in a store. So the business design relies on small first batches and fast recuts rather than a perfect forecast.

## Results (modelled)

| Metric | Before | After |
| --- | --- | --- |
| Trend-to-store cycle | 14 weeks | 7.7 weeks (committed: under 8) |
| Designer creative time | 37% | 63% |
| Concept approval | 15 working days | 4 days |
| Design headcount change | n/a | 0 (zero layoffs) |
| Year-2 net value | n/a | ₹81 Cr on a ₹38 Cr investment |

## Notes and limitations

- Store, sales and inventory data are **synthetic**, generated for this project. Weather and search-trend data are real.
- Business results are **modelled** from case figures and stated assumptions; they are not observed outcomes.
- Design drafts in the copilot are simulated to demonstrate the human-AI workflow.
- In production, the model would be retrained on real transaction data, with drift monitoring and authentication added.

## About

Built for the PwC Challenge 8th Edition (Round 2, Workstream 1: Design & Product Development).
