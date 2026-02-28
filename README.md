# hedgefund-ai

An **AI-powered SaaS** platform designed to monitor and analyze hedge fund portfolios by parsing SEC filings. The system transforms raw 13F, 13D/G, and Form 4 data into actionable investment intelligence, leveraging multiple AI/ML models to surface promising stocks and portfolio trends.

## Table of Contents
1. [Overview](#overview)
2. [Key Features](#key-features)
3. [Getting Started](#getting-started)
   - [Configuration](#configuration)
   - [Running the Application](#running-the-application)
4. [Usage Guide](#usage-guide)
   - [Command Line Interface](#command-line-interface)
   - [Data Update Workflow](#data-update-workflow)
   - [AI Model Integration](#ai-model-integration)
5. [Architecture](#architecture)
6. [Deployment (SaaS)](#deployment-saas)
7. [Contribution](#contribution)
8. [License](#license)

---

## Overview
**hedgefund-ai** is a Python-based toolkit for tracking, analyzing, and interpreting institutional investment activity. By combining automated SEC scraping with advanced NLP/AI, the project provides:

- A complete database of hedge fund filings (quarterly and non-quarterly)
- Industry classification (GICS) for enhanced sector insights
- Multi-model AI analysis for stock discovery and due diligence
- Command-line interface suitable for integration into SaaS pipelines

## Key Features
-  **Full SEC 13F / 13D/G / Form 4 coverage** with historical and live updates
-  **AI-powered stock scoring** using customizable model providers (GitHub Models, Google Gemini, Hugging Face, etc.)
-  **Industry context** via an internal GICS parser built from official standards
-  **Ticker resolution** from CUSIP using intelligent fallbacks (yfinance  Finnhub  FinanceDatabase)
-  **CSV-driven data management** for easy customization of funds and AI models
-  **CLI menu system** allowing fund, quarter, or ticker-centric reports
-  **Auto-update utilities** for sorting and maintaining dataset integrity

## Getting Started
Prepare your environment, install dependencies, and run the tool locally.

**Installation**
Clone the repository and install required packages:
`ash
git clone <your-repo-url>
cd hedge-fund-tracker
pipenv install --dev
# or, if using venv:
# python -m pip install -r requirements.txt
`

### Configuration
Copy and edit the environment template before running the application:
`ash
cp .env.example .env
# edit .env to include any API keys you intend to use
# FINNHUB_API_KEY, GITHUB_TOKEN, GOOGLE_API_KEY, GROQ_API_KEY, HF_TOKEN, OPENROUTER_API_KEY
`
API keys are optional; the system functions with yfinance for most operations, but certain features (e.g. multiple AI providers or CUSIP lookups) work better with valid tokens.

### Running the Application
Activate the virtual environment, then launch the CLI:
`ash
pipenv shell           # enter the pipenv environment
python -m app.main     # start the interactive menu
`

Upon startup, you'll be presented with analysis options such as viewing recent filings, quarter summaries, individual fund/stock reports, or invoking the AI analyst.

## Usage Guide
### Command Line Interface
The main menu offers numeric commands:
`
0. Exit
1. View latest non-quarterly filings activity (13D/G, Form 4)
2. Analyze overall hedge-funds stock trends for a quarter
3. Analyze a specific fund's quarterly portfolio
4. Analyze a specific stock's activity for a quarter
5. Run AI Analyst to find most promising stocks
6. Run AI Due Diligence on a stock
`
Follow prompts to select funds, quarters, or tickers; the output is formatted for readability.

### Data Update Workflow
Use the updater script to refresh datasets:
`ash
python -m database.updater
`
The updater menu enables bulk or individual 13F generation and non-quarterly filing retrieval. It also automatically sorts database/stocks.csv and syncs documentation.

### AI Model Integration
AI configuration is driven by CSV files under database/ and prompts in pp/ai/prompts. You can add or swap models by editing models.csv and providing corresponding API credentials in .env.

## Architecture
- **app/**  core logic, CLI, AI agents
- **analysis/**  financial evaluation modules
- **database/**  CSV data, updater tools, GICS parsing
- **scraper/**  EDGAR and XML scraping utilities
- **stocks/**  price fetchers, ticker resolvers, 3rd-party clients
- **utils/**  generic helpers for console, database, and string operations

## Deployment (SaaS)
This project is designed for easy containerization. Typical steps:

1. Create a Dockerfile using the Python base image
2. Mount or provision persistent storage for database/
3. Supply .env via environment variables or secret store
4. Iterate with a CI/CD pipeline to periodically run the updater and commit changes

> _Details coming soon_: future updates will include a Helm chart and sample docker-compose configuration.

## Contribution
Contributions are welcome! Please:
- Fork the repository and create a topic branch
- Adhere to lack formatting and run existing tests
- Submit PRs with clear descriptions and reference any related issues

For major changes or ideas, open an issue first so we can discuss design implications.

## License
Released under the **MIT License**  see [LICENSE](LICENSE) for details.
