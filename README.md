# GCC Atlas — India GCC Intelligence MVP

**A decision-support prototype for teams evaluating India GCC market entry.**

This repository contains a React + Vite + Tailwind frontend, Flask REST API, and MySQL schema/seed data. The MVP is designed to be runnable locally and deliberately labels seeded records as demo data.

## Features
- GCC landscape explorer with text search and city/sector filters
- City comparison with adjustable priorities (illustrative demo indicators)
- GCC pipeline signal ledger with explicit weights and source links
- Provider directory with category/city filters and evidence labels
- Provider engagement ledger with source links and self-reported outcomes
- State incentive explorer with policy links and verification dates
- Market-entry brief combining city priorities, pipeline leads and policy research leads
- Source/provenance fields on records; seeded records are marked `DEMO`
- CRUD endpoints for GCCs, providers and incentives

## Product deck and research

See [DECK.md](./DECK.md) for the problem framing, stakeholder map, opportunity prioritisation, solution, positioning, prototype limits, pilot plan and linked research references. Market figures are dated and attributed; research estimates, demo inputs and hypotheses are kept distinct.

## Architecture
`React/Vite → Flask REST API → MySQL`

## Prerequisites
- Node.js 20+
- Python 3.10+
- MySQL 8+

## Run locally

### 1. Database
```bash
mysql -u root -p < server/sql/schema.sql
```

### 2. API
```bash
cd server
copy .env.example .env
python -m pip install -r requirements.txt
python app.py
```
Edit `.env` with your MySQL credentials. API defaults to `http://localhost:4000`.

### 3. Frontend
In another terminal:
```bash
cd client
npm install
npm run dev
```
Open the Vite URL (usually `http://localhost:5173`).

## Demo mode
The frontend includes a built-in, clearly labelled demo dataset fallback if the API is not reachable, so the interface can still be evaluated. When the API is reachable it loads data from Flask/MySQL. Set `VITE_API_URL` in the client environment when the API is hosted at a URL other than `http://localhost:4000`.

City weights, pipeline edits and provider evidence submissions are stored in the current browser's local storage. They do not sync to the Flask API or across users. Provider responses are self-reported; an attached URL is not independent verification. Pipeline scores represent signal coverage, not the probability of a GCC launch.

## API routes
- `GET /api/health`
- `GET /api/gccs?q=&city=&sector=`
- `POST /api/gccs`
- `PATCH /api/gccs/:id`
- `DELETE /api/gccs/:id`
- `GET /api/providers?q=&city=&category=`
- `POST /api/providers`
- `PATCH /api/providers/:id`
- `DELETE /api/providers/:id`
- `GET /api/incentives?state=`
- `POST /api/incentives`
- `GET /api/cities`
- `GET /api/brief?city=&headcount=&sector=`

## Data integrity / research note
The seeded GCCs and providers are fictional examples for product demonstration, not a verified list of actual companies or vendors. City scores are illustrative. Policy entries are research leads and do not establish current eligibility or entitlement. The overview's FY2026 landscape totals are attributed to the NASSCOM–Zinnov report, with date and unit-of-count caveats in [DECK.md](./DECK.md). Before production use, add source ingestion, human review, change history and legal review of policy eligibility. Do not treat city scores as investment advice.

## Suggested 48-hour build sequence
1. Run app and verify the core workflow.
2. Replace demo GCC/provider records with verified records and citations.
3. Interview 5–8 market-entry consultants and GCC leaders; measure research time and willingness to pay.
4. Add CSV export, saved workspaces and source change alerts after validating demand.
