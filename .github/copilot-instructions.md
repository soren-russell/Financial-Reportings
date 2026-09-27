# Copilot instructions

## Project purpose

- This is a learning and portfolio project for financial data analysis and data engineering.
- Build tools that collect, validate, store, analyze, and explain financial data.
- This is not a trading bot. Do not add buy/sell/hold recommendations, personalized investment advice, or unsupported price predictions.
- Distinguish implemented features from planned features in code, reports, and documentation.

## Working with the learner

- Explain important decisions and unfamiliar financial or programming terms in plain language.
- Prefer small, focused changes over large rewrites. State assumptions before relying on them.
- Follow the repository's existing structure and conventions. Use Python's standard `snake_case` naming.
- Target Python 3.11 or newer unless the project configuration specifies otherwise.
- Avoid adding dependencies unless they are needed; explain why and update the dependency manifest when adding one.
- Never put API keys, credentials, or other secrets in source code or committed files. Read credentials from environment variables when needed.
- For meaningful behavior changes, add or update tests and directly related documentation.
- After changes, run the smallest relevant existing test command. The current test command is `python -m unittest discover -s tests -v`. Do not claim tests passed unless they were run successfully.

## Financial-data correctness

- Treat financial data as time- and source-dependent. Preserve the provider/source, instrument or series identifier, observation date (the date the value describes), retrieval timestamp, units, and frequency where applicable. Do not substitute the retrieval date for the observation date.
- State whether equity returns use close, adjusted close, or total-return data. These are different measures; do not describe one as another.
- Define return windows consistently. When a task says "weekly," establish whether that means five trading observations or seven calendar days; document and test the chosen convention. Align compared series to the same dates.
- Label units in stored fields, calculations, and reports. A basis point is 0.01 percentage point; convert yield changes carefully and distinguish basis points from percent or percentage points.
- Define yield spreads with their order and unit (for example, 10-year minus 2-year, in basis points). Do not silently mix percentage-point and basis-point values.
- Compare a sector proxy and benchmark over identical observation dates and compatible return definitions. Identify ETFs as proxies, not as the indexes or underlying sectors themselves.
- Describe volatility as a historical estimate over a specified window and method. It is not a forecast or a direct probability of loss.
- Treat correlation as co-movement over a specified sample, not evidence that one variable caused another. Separate observed results from possible explanations and state important limitations.
- For macroeconomic data, distinguish the period measured, release/publication date, retrieval timestamp, initial estimate, and subsequent revisions when the source makes this information available. Do not silently overwrite vintage data if revision analysis is in scope.
- Preserve missing or invalid observations as missing or flagged; do not replace them with zero or invent values. Surface data gaps, stale data, provider errors, and calculation assumptions.
- Prefer authoritative source documentation for definitions, release schedules, units, revisions, and licensing. Respect provider terms and rate limits.

## Review checklist for data and analysis changes

Before considering financial-data work complete, check:

1. Is the correct instrument or series being collected, and is it accurately named?
2. Are source, observation date, retrieval time, frequency, and units clear?
3. Are return/yield formulas and window conventions explicit and tested?
4. Are time-series comparisons aligned to compatible dates and definitions?
5. Are missing values, stale observations, and revisions handled transparently?
6. Does the explanation avoid unsupported causal claims or trading recommendations?
