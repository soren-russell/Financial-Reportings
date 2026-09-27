# Financial Market Analytics

A learning-focused financial data project for building practical skills in data analysis, Python, GitHub, and GitHub Copilot. The goal is to develop a reliable pipeline that collects, stores, analyzes, and explains market and economic data—not to build a trading bot.

This repository is a work in progress. The current code collects selected market data from Yahoo Finance and stores summary records in SQLite. Macroeconomic data collection, interest-rate analysis, and individual-stock analysis are planned, not implemented yet.

> **Not investment advice:** This project is for education and analysis. It does not make predictions or provide buy, sell, or hold recommendations. Financial data can be delayed, revised, incomplete, or incorrect; verify important values with authoritative sources.

## Questions this project is working toward

- What is happening across major U.S. markets?
- Which sectors are outperforming or underperforming the S&P 500?
- How are interest rates, Treasury yields, and the yield curve changing?
- How might rate changes relate to equity sectors, credit markets, and other assets?
- How has market volatility changed?
- What macroeconomic conditions may be relevant to current market movements?
- How is an individual stock performing relative to its sector and the broad market?
- What relationships appear among market and economic variables?

These are descriptive and exploratory questions. Correlation can help identify relationships worth investigating, but does not establish causation.

## Current project status

### Implemented

- Market-data collection through `yfinance` for configured index/sector proxies, VIX, commodities, and currency pairs.
- SQLite tables for market snapshots and additional planned data categories.
- Basic return, volatility, spread, relative-performance, and 52-week-extreme calculation utilities.
- A command-line interface for collection and reports.
- An analysis module with snapshot and sector-ranking queries.

### Important limitations

- The configured index symbols are exchange-traded funds, not the indexes themselves: `SPY` tracks the S&P 500, `QQQ` tracks the Nasdaq-100 (not the Nasdaq Composite), `DIA` tracks the Dow, and `IWM` tracks the Russell 2000. Their traded prices and returns are proxies and can differ from index levels and returns.
- Sector data uses sector ETFs as proxies for their S&P 500 sectors.
- The current collector stores latest summary records; it does not yet persist a complete daily OHLCV history for all assets.
- Federal Reserve Economic Data (FRED) series are listed in configuration, but a FRED collector is not implemented.
- Market data providers may revise, delay, or restrict data. Check provider terms and timestamps before relying on results.

## Data roadmap

The intended pipeline will grow in stages:

1. **Market data:** broad-market benchmarks, all 11 sector groups, VIX, commodities, and foreign exchange.
2. **Macroeconomic data:** federal funds rate, Treasury yields, CPI/Core CPI, PCE/Core PCE, unemployment, payrolls, jobless claims, wages, GDP, retail sales, and industrial production.
3. **Credit markets:** investment-grade and high-yield spreads when reliable data sources are available.
4. **Analysis:** daily/weekly/monthly/YTD returns, yield changes in basis points, yield-curve spreads, volatility, benchmark-relative sector performance, and exploratory relationships among variables.
5. **Individual stocks:** performance compared with a relevant sector proxy and broad-market benchmark.
6. **News and reporting:** a Financial News Agent, source-aware summaries, charts, and repeatable reports.

For each series, record its provider, symbol/series ID, observation date, retrieval time, units, frequency, and—where relevant—release date and revision status. Keep those details with the data so results can be traced and interpreted correctly.

## Repository layout

```text
.
├── agents/
│   ├── analysis_engine.py  # Queries and summaries
│   ├── market_agent.py     # Market-data collection
│   └── orchestrator.py     # Coordinates collection
├── config/
│   └── data_config.py     # Symbols and data-series mappings
├── data/                  # Local SQLite database (created at runtime)
├── notebooks/
│   └── examples.py        # Exploratory analysis examples
├── tests/
│   └── test_calculations.py
├── utils/
│   ├── calculations.py
│   └── database.py
├── main.py                # Command-line entry point
└── requirements.txt
```

## Set up locally

Use Python 3.9 or later. In PowerShell:

```powershell
git clone https://github.com/soren-russell/Financial-Reportings.git
cd Financial-Reportings
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

If PowerShell blocks virtual-environment activation, you can skip activation and use the environment's Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe main.py collect
```

On macOS/Linux, create and activate the environment with:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

## Run the project

Collect configured market data:

```bash
python main.py collect
```

Generate reports:

```bash
python main.py analyze --report summary
python main.py analyze --report indexes
python main.py analyze --report sectors
python main.py analyze --report vix
python main.py analyze --report commodities
python main.py analyze --report forex
```

Collect data and print a summary:

```bash
python main.py full
```

Run the unit tests:

```bash
python -m unittest discover -s tests -v
```

The first data-collection run requires internet access and may take time because it requests data for multiple symbols. Review the provider's terms and the returned observation dates. Do not treat a successful command as proof that every symbol returned fresh data.

## Learn to use GitHub Copilot on this project

GitHub Copilot works best as a coding partner when you give it a narrow task, the relevant files, constraints, and a way to verify the result. You remain responsible for checking generated code and financial calculations.

### A useful workflow

1. **Explore before editing.** Ask Copilot to explain a small part of the repository and identify assumptions.
2. **Define one outcome.** For example, add a test for one return calculation—not “finish the whole macro pipeline.”
3. **Give domain context.** Specify the instrument, source, units, observation frequency, and how missing or revised observations should be handled.
4. **Ask for tests and documentation.** Request tests for normal, boundary, and missing-data cases.
5. **Review the diff.** Check the code, SQL, units, dates, provider behavior, and error handling yourself.
6. **Run the tests and inspect the output.** Compare a small sample with the source provider before trusting a result.
7. **Commit in small steps.** Use clear commit messages so you can understand and undo changes later.

### Example Copilot prompts

Use prompts like these in Copilot Chat or as task descriptions:

> Explain how `agents/market_agent.py` currently collects and stores data. Do not change files. Point out where the observed market date differs from the collection date.

> Add unit tests for weekly returns using a small deterministic price series. Define whether a “week” means five trading observations or seven calendar days, state the assumption, and do not change unrelated code.

> Propose a FRED data-ingestion design for the 10-year Treasury yield. Before coding, identify the FRED series ID, units, observation frequency, release/revision considerations, and secret-management needs. Do not claim the collector is implemented until tests pass.

> Review this change for financial-data correctness. Check symbols versus index names, percentage versus basis-point units, trading-day versus calendar-day windows, missing values, date alignment, and whether the tests cover the stated behavior.

> Create a SQL query that compares sector ETF returns with the S&P 500 proxy over the same observation dates. Explain the join keys and limitations; do not interpret correlation as causation.

For broader tasks, ask Copilot to make a short plan first. Approve or adjust the plan, then implement and test one slice at a time.

## Prompting AI for financial analysis

Treat an AI-generated financial summary as a draft analysis, not a source of truth. Good prompts provide data and methodology, define terms, request uncertainty, and prohibit unsupported causal claims.

### Prompt template

```text
Analyze the attached [frequency] data for [date range].

Sources and fields:
- [series/symbol]: [provider], [units], [observation frequency]
- [benchmark]: [provider], [units], [observation frequency]

Method:
- Define returns as [formula and period convention].
- Align observations by [date rule].
- Report missing, stale, or revised observations.

Please:
1. Summarize the observed changes with dates and units.
2. Compare [assets/sectors/indicators] using the same time window.
3. Separate direct observations from possible explanations.
4. Identify limitations and alternative explanations.
5. Do not infer causality from correlation or give trading recommendations.
```

Avoid prompts such as “Will the market go up?” or “What should I buy?” Prefer questions that can be answered from known data, such as “How did the 10-year Treasury yield and rate-sensitive sector proxies move over this period, and what limitations apply to that comparison?”

## Financial data analysis concepts to practice

- **Observation date vs. retrieval date:** the date a value describes is not necessarily the date you downloaded it.
- **Price return:** `(ending price / beginning price - 1) * 100`. State whether the calculation uses adjusted close, close, or total return; they are not interchangeable.
- **Return windows:** “weekly” may mean five trading sessions or seven calendar days. Define a convention and use it consistently.
- **Basis points:** one basis point is 0.01 percentage point. A yield moving from 4.20% to 4.35% changes by 15 basis points.
- **Yield spreads:** for example, `10-year yield - 2-year yield`, expressed either in percentage points or basis points. Label the unit.
- **Sector-relative return:** compare sector and benchmark returns over identical dates and using compatible return definitions.
- **Volatility:** a historical estimate based on past returns, not a forecast or a direct measure of the probability of loss.
- **Correlation:** a measure of co-movement over a chosen sample; it does not show that one variable caused another.
- **Macroeconomic releases:** distinguish the period measured, release date, first estimate, and later revisions.

## Data quality and responsible use

- Store source and units with observations; never silently combine incompatible frequencies or units.
- Preserve missing values as missing rather than replacing them with zero.
- Validate dates, duplicate records, unexpected gaps, and large changes before summarizing.
- Prefer official sources for macroeconomic series (for example, FRED and the underlying statistical agencies); keep the source citation and series ID.
- Use the appropriate instrument: ETFs are proxies for indexes/sectors, and their returns may include tracking differences, fees, distributions, and market-price effects.
- Do not publish API keys, `.env` files, local databases, or other secrets. If API credentials are added later, load them from environment variables and exclude local secret files from Git.
- Respect provider licensing, terms of use, and rate limits.

## Future architecture

The intended system separates collection, storage, analysis, and reporting:

```text
Market Data Agent ───────┐
Macroeconomic Collector ─┼──> validated observations ──> database ──> analysis/reporting
Financial News Agent ────┤
Stock Analysis Agent ───┘
```

The Market Data Agent is the first component. The Financial News Agent and Individual Stock Analysis Agent are future components; they are not currently part of this implementation.

## Contributing and learning

This is a portfolio learning project. Good next contributions include correcting/clarifying benchmark symbols, persisting dated historical observations, adding a tested FRED collector, and building analysis that clearly documents its assumptions. Open an issue or submit a pull request with a focused change and reproducible test.

## License

No license is currently specified. Add a `LICENSE` file before presenting the project as open source or granting reuse rights.

## About

Built as a portfolio project to practice financial data analysis, Python, SQL, Git, and responsible use of GitHub Copilot and AI tools.
