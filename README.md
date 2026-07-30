# BucketsWealth

Free financial decision tools for well-earning millennials. Live at [www.bucketswealth.com](https://www.bucketswealth.com).

Four calculators in a single static page:

- **Rent vs Buy** — 20-year cost comparison including equity build, the opportunity cost of your down payment, rent escalation, property tax, maintenance and lifestyle add-ons
- **RSU Sell vs Hold** — priority-ordered decision engine covering high-interest debt, emergency fund, concentration risk and long-term capital gains timing
- **Auto Loan Optimizer** — pay off vs accelerate vs invest, based on real amortization
- **Compound Interest Calculator** — nominal and inflation-adjusted growth projections

RSU and Auto Loan also include a Monte Carlo mode that runs 1,000 simulated futures and reports outcome probabilities.

## Tech

Static site, no build step, no backend. Everything lives in `index.html` (HTML, CSS and vanilla JS inline). Chart.js and Inter are loaded from CDNs. Hosted on GitHub Pages.

- `index.html` — all four tools
- `feedback.html` — embedded feedback form

## Disclaimer

For educational purposes only. Not financial advice. Tax constants reflect US federal rules and need periodic review.
