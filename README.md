# IndexForge AI

**AI-Powered Crypto Index Builder & Portfolio Intelligence Platform**

> Turn an investment idea into a transparent, rules-based crypto index with explainable portfolio construction, historical simulation, benchmark comparison, and rebalancing insights.

**Project:** SoSoValue WaveHack Buildathon — Wave 3  
**Status:** In development / MVP  
**Category:** AI · Crypto market intelligence · Portfolio analytics · Index construction

> [!NOTE]
> This README describes the intended MVP and development setup. Features marked as planned are not claims that they are already implemented. Buildathon eligibility, dates, and API entitlements should be confirmed using the official program resources.

## Overview

IndexForge AI is a platform for designing and evaluating customized cryptocurrency indexes. Users describe an investment theme in natural language—for example, *“Build a moderately risky AI infrastructure index with five tokens and a maximum 30% weight per asset.”* The application translates that request into explicit screening and weighting rules, then uses supported market data to calculate and explain a hypothetical index.

The goal is to make thematic crypto index research **transparent, reproducible, and accessible**, without presenting AI-generated allocations or performance claims as verified financial results.

## Planned MVP Features

- **AI Index Builder:** Convert plain-English prompts into validated index specifications.
- **Asset Screening:** Filter supported assets by sector/theme, market cap, liquidity, and data availability.
- **Transparent Weighting:** Equal-weight, market-cap-weighted, and capped market-cap approaches.
- **Portfolio Dashboard:** Asset composition, weight breakdowns, and visual analytics.
- **Historical Backtesting:** Simulated performance with explicit assumptions, rebalancing rules, and transaction-cost estimates.
- **Benchmark Comparison:** Compare custom indexes against accessible SoSoValue SSI benchmarks when the required data is available.
- **Risk Analytics:** Volatility, drawdown, concentration, and related portfolio statistics.
- **Rebalancing Assistant:** Identify weight drift and propose reviewable, non-automatic allocation changes.
- **Saved Strategies:** Persist index configurations and simulation results after authentication is integrated.

**MVP approach:** Research, index design, and simulation first. Wallet connections and trade execution are optional future enhancements, not prerequisites.

## Tech Stack

| Area | Technology |
| --- | --- |
| Frontend | Next.js (App Router), React, TypeScript |
| Styling and components | Tailwind CSS, shadcn/ui, Lucide React |
| Data visualization | Recharts, TradingView Lightweight Charts, D3 |
| Client-side state | Zustand |
| Server state and HTTP | TanStack Query, Axios |
| Forms and validation | React Hook Form, Zod |
| AI | OpenAI API (server-side only) |
| Market intelligence | SoSoValue API, subject to access and endpoint availability |
| Authentication and storage | Supabase Auth and PostgreSQL |
| Analytics backend (planned) | Python, FastAPI, Pydantic, Pandas, NumPy |
| Testing | Vitest, Testing Library, Playwright; Pytest for backend |
| Deployment (planned) | Vercel (frontend), Railway (backend), Supabase |
| Optional Web3 support | wagmi, viem |

## Repository Layout

The **Next.js app lives directly in `frontend/`**, not in `frontend/indexforge/` or another nested folder.

```text
indexforge/
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── app/
│   │   │   ├── dashboard/
│   │   │   ├── create/
│   │   │   ├── indexes/
│   │   │   ├── backtest/
│   │   │   ├── compare/
│   │   │   └── settings/
│   │   ├── components/
│   │   │   ├── ui/
│   │   │   ├── layout/
│   │   │   ├── dashboard/
│   │   │   ├── charts/
│   │   │   ├── index-builder/
│   │   │   ├── portfolio/
│   │   │   └── rebalancing/
│   │   ├── features/
│   │   ├── hooks/
│   │   ├── lib/
│   │   ├── store/
│   │   ├── types/
│   │   └── config/
│   ├── .env.local              # local only; never commit
│   └── package.json
├── backend/                    # planned FastAPI service
├── docs/                       # planned documentation
└── README.md
```

Some paths above are **target structure** and may not exist until their features are implemented.

## Getting Started

### Prerequisites

- Node.js (current supported LTS version recommended)
- npm
- Python 3.11+ **when adding the backend**
- Git
- Optional: Supabase project, OpenAI API key, and approved SoSoValue API access

### 1. Clone the repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd indexforge
```

Replace `<YOUR_REPOSITORY_URL>` with the actual Git repository URL. If you already have the project locally, simply open its root folder.

### 2. Create the frontend (only if not already initialized)

From the `indexforge` root:

```bash
npx create-next-app@latest frontend --typescript --tailwind --eslint --app --src-dir --import-alias "@/*" --use-npm
```

**If `frontend/package.json` already exists, do not run `create-next-app` again.** Continue with the next step.

### 3. Install dependencies

```bash
cd frontend
npm install
```

For a freshly initialized project that does not yet list the planned feature packages, install them as needed:

```bash
# UI and utilities
npm install lucide-react clsx tailwind-merge class-variance-authority

# Financial charts
npm install recharts lightweight-charts d3
npm install -D @types/d3

# State management and HTTP
npm install zustand @tanstack/react-query axios

# AI, forms, and input validation
npm install openai zod react-hook-form @hookform/resolvers

# Auth and storage
npm install @supabase/supabase-js @supabase/ssr

# Dates, animations, and notifications
npm install date-fns framer-motion sonner

# Test tooling
npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom @playwright/test
```

Initialize the component library if needed:

```bash
npx shadcn@latest init
npx shadcn@latest add button card input label textarea select tabs table badge separator dialog dropdown-menu sheet sidebar skeleton progress tooltip alert form switch slider sonner chart
```

For browser tests, run `npx playwright install chromium`.

### 4. Configure local environment

Create `frontend/.env.local`:

```dotenv
NEXT_PUBLIC_APP_NAME="IndexForge AI"
NEXT_PUBLIC_API_URL="http://localhost:8000"
NEXT_PUBLIC_SUPABASE_URL=""
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=""

# Server-side only. Prefer storing these in backend/.env once FastAPI is added.
OPENAI_API_KEY=""
SOSOVALUE_API_KEY=""
```

**Security requirements:**

- Never commit `.env.local`, `backend/.env`, or API secrets.
- Never prefix private API keys with `NEXT_PUBLIC_`.
- Access OpenAI and SoSoValue credentials only from trusted server-side code.
- Configure Supabase Row Level Security before allowing users to save private data.
- Empty example values are placeholders, not working credentials.

### 5. Run the frontend

```bash
npm run dev
```

Open **http://localhost:3000**.

### 6. Verify the build

```bash
npx tsc --noEmit
npm run lint
npm run build
```

The lint command assumes the generated `package.json` contains an appropriate `lint` script. If not, configure ESLint or use `npx eslint .`.

## Backend Setup (Planned)

The Python service will own asset eligibility logic, portfolio math, historical simulations, and server-to-server API calls. When the backend has been created:

```bash
cd backend
python -m venv .venv
# Windows PowerShell:
.venv\Scripts\Activate.ps1
# macOS/Linux:
# source .venv/bin/activate

pip install fastapi "uvicorn[standard]" pydantic pandas numpy httpx python-dotenv pytest
uvicorn app.main:app --reload --port 8000
```

These commands require an implemented `backend/app/main.py`; the backend is not generated by the frontend setup instructions.

## How Index Creation Will Work

1. **Describe a theme:** e.g., “Create a five-asset DeFi index with moderate risk.”
2. **Interpret the prompt:** Generate a structured, schema-validated index configuration.
3. **Screen assets:** Apply data-backed eligibility rules and document exclusions.
4. **Calculate weights:** Use deterministic functions, not LLM-invented percentages.
5. **Simulate performance:** Use historical data without look-ahead bias, with clearly documented rebalance dates and costs.
6. **Compare benchmarks:** Display supported SSI comparisons when trustworthy benchmark series are available.
7. **Review rebalancing:** Show suggested target changes for user review; never execute automatically in the MVP.

### Illustrative Index Configuration

```json
{
  "name": "AI Infrastructure Index",
  "theme": "AI & Infrastructure",
  "assetCount": 5,
  "riskProfile": "moderate",
  "weighting": "capped_market_cap",
  "maxAssetWeight": 0.3,
  "rebalanceFrequency": "monthly"
}
```

This is an **example configuration**, not a verified list of investable assets or a live index.

## Financial Methodology

For an index of `N` assets:

**Equal weighting:**

```text
weight_i = 1 / N
```

**Market-cap weighting:**

```text
weight_i = market_cap_i / sum(market_cap_j)
```

**One-period index return (start-of-period weights):**

```text
portfolio_return_t = sum(weight_i_(t-1) * asset_return_i_t)
```

The backend should enforce valid weights, positive/eligible asset values, concentration caps, data-quality checks, and explicit index reconstitution rules. Backtests should disclose fees, missing data, survivorship and look-ahead bias, and whether any benchmark is directly comparable.

## SoSoValue Integration

IndexForge AI is designed to use SoSoValue market intelligence where permitted by the API subscription and documentation.

- [SoSoValue Developer Console](https://sosovalue.com/developer/dashboard)
- [SoSoValue API Documentation](https://sosovalue.gitbook.io/soso-value-api-doc/)
- [SoSoValue Indexes](https://ssi.sosovalue.com/)

API datasets, coverage, endpoint behavior, access permissions, and rate limits must be confirmed before integrating individual features. If an endpoint is unavailable, the UI should clearly indicate that rather than silently portraying invented data as live.

## Testing Plan

- **Unit tests:** Weight totals, eligibility filters, caps, returns, and rebalancing calculations.
- **API tests:** Schema validation, authentication boundaries, market-data errors, and cache behavior.
- **Component tests:** Index-builder forms, loading/error states, and charts.
- **End-to-end tests:** Prompt → validated index → simulation → comparison → saved configuration.

## Roadmap

- [ ] Scaffold and verify Next.js frontend
- [ ] Build responsive dashboard shell and navigation
- [ ] Connect approved SoSoValue market-data endpoints
- [ ] Implement AI prompt-to-index schema
- [ ] Implement deterministic screening and weighting
- [ ] Build allocation and risk analytics dashboards
- [ ] Add backtesting and SSI benchmark comparisons
- [ ] Add rebalancing previews
- [ ] Integrate authentication and saved strategies
- [ ] Add automated tests, deployment, and demo walkthrough
- [ ] Evaluate optional wallet connectivity and on-chain workflows

## Buildathon Submission Checklist

Check the official WaveHack rules for the latest submission requirements. The supplied project brief identifies the following expected deliverables:

- Public GitHub repository and clear setup instructions
- Public live demo URL
- Project overview and team information
- Wave progress changelog
- Short project introduction/demo video (recommended)
- Meaningful SoSoValue integration and functional demonstration

Add deployment URLs, demo footage, and competition-specific details once available.

## Responsible Financial Use

IndexForge AI is intended for **research and educational simulation**, not investment advice. Past or simulated performance does not guarantee future returns. Crypto assets involve substantial volatility, liquidity, custody, and market risks. Suggested portfolios require independent evaluation. No live trading or automatic asset custody is part of the initial MVP.

## Contributing

Issues and pull requests are welcome as development progresses. When submitting changes, include a concise explanation, relevant tests, and screenshots for major UI changes.

## License

**Not yet specified.** Add a `LICENSE` file before describing this repository as open-source or authorizing reuse under a particular license.

---

**IndexForge AI** — Design indexes with AI. Validate them with data. Understand the risks.
