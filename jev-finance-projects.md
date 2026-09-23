# Jev (TypeSafe System One) — Finance & Trading Projects

Projects using [Jev](https://typesafe.ai), TypeSafe AI's System One decision model (released 2026-09-15), in investment, trading, and financial-data contexts. Surveyed 2026-09-20 via GitHub API and community awesome-lists.

## Reference project

- **[jarrodwatts/jev-trader](https://github.com/jarrodwatts/jev-trader)** (★1.3k, 2026-09-16) — One AI trade decision every Monad block (~300 ms). Jev reads the Kuru MON-USDC order book and answers buy or sell; the bot posts a post-only limit order one tick inside the touch, earning the spread. Bun/TypeScript, dry-run mode, SSE dashboard. The template most projects below derive from.

## Live trading / trading systems

| Repository | Created | Description |
|---|---|---|
| [jarrodwatts/jev-trader](https://github.com/jarrodwatts/jev-trader) | 9/16, ★1.3k | Monad/Kuru MON-USDC market maker. Jev decides buy/sell every ~300 ms block; post-only limit orders. Bun/TS, dry-run + SSE dashboard |
| [aowang-ai/jev-trade](https://github.com/aowang-ai/jev-trade) | 9/17, ★28 | jev-trader ported to Hyperliquid. Five isolated sleeves (BTC/ETH/SOL/DOGE/BNB), five wallets, real fills. Jev answers long/short then open/close/hold via Choice. [Live desk](https://www.jev-trade.com/) |
| [OpenByteInc/QuantDinger](https://github.com/OpenByteInc/QuantDinger) | repo 2025, ★11.7k | Open-source AI Trading OS (crypto, stocks, forex). Added Jev post-release as a "pre-trade decision gate" in front of the LLM gate. Largest Jev integration by stars |
| [buberlo/jev-trader](https://github.com/buberlo/jev-trader) | 9/19 | Python redesign. Feature engine → <400-token state → one Jev call with six atomic judgments (regime, direction, toxic_flow, liquidity_stressed, quote_environment, inventory_pressure) → policy engine → hard risk vetoes → execution → calibration log. Most rigorous architecture |
| [zzsong1023/jev-market-reflex](https://github.com/zzsong1023/jev-market-reflex) | 9/19 | Kraken BTC/ETH/SOL live data → Jev BUY/SELL/HOLD → simulated paper portfolio. Bun/TS, execution fully simulated |
| [rnjsxodyd90/jev-trading-bot](https://github.com/rnjsxodyd90/jev-trading-bot) (Paperline) | 9/19 | Cash-funded spot paper trading ($1,000 virtual) on read-only Kuru MON/USDC data. Detailed risk limits (loss halt, allocation caps, spread caps). Jev adapter optional |
| [UditJain2622004/Jev-Trading](https://github.com/UditJain2622004/Jev-Trading) | 9/19 | Python. SOL backtest experiments with Jev (grid / buy-the-dip strategy logs and result CSVs). No README |
| [beto11-gif/jev-trading-backend](https://github.com/beto11-gif/jev-trading-backend) | 9/18 | Binance Spot market-data backend (Node 24/Fastify). Jev integration explicitly disabled via `DisabledJevAnalyzer` pending confirmed docs |

## Forecasting, evaluation & financial data processing

| Repository | Created | Description |
|---|---|---|
| [sosopop/jev_stock](https://github.com/sosopop/jev_stock) | 9/17, ★7 | Hong Kong stock direction forecasting. AKShare fetches target + HSI + HSTECH; Jev answers up/flat/down as Choice; standalone HTML reports. Careful treatment of probability calibration |
| [Gamma-Software/jev-signals-lab](https://github.com/Gamma-Software/jev-signals-lab) | 9/19 | Paper-only signals research POC. One market snapshot → 12 independent Jev questions → in-code rule engine (BUY gate: trend>.75 AND momentum>.70 …) |
| [IslamBaraka90/jev-typesafe-real-financial-use-cases](https://github.com/IslamBaraka90/jev-typesafe-real-financial-use-cases) | 9/19 | "Jev Lab": 50 financial demos (ledgers, fraud, wallets, portfolios, trades, filings, strategies) plus a backtest lab replaying Jev trade decisions on daily candles |
| [simonmesmith/jev-banking77-experiment](https://github.com/simonmesmith/jev-banking77-experiment) | 9/18 | BANKING77 banking-intent classification: 92.40% accuracy vs 93.66% for fine-tuned BERT (−1.26 pt), US$0.44 total test cost |
| [adilmoujahid/jev-banking77-demo](https://github.com/adilmoujahid/jev-banking77-demo) | 9/19 | Single-page Next.js app classifying BANKING77 customer-support queries with Jev |
| [kyotofin/tax-doc-classifier](https://github.com/kyotofin/tax-doc-classifier) | release week | IRS tax-document page classifier: 261 forms, 100% strict accuracy on their corpus, ~$0.001/page (34× cheaper, 6× faster than their prior LLM pipeline) |

## Empty / placeholder repos

- `maxlibin/moomoo-jev-trader` (Moomoo dashboard, intended), `Pastorkid/jevTradingBoth`, `hifizz/jev-finance-benchmark` — created but no content yet.

## Observed patterns

1. **"Jev judges, code executes" is universal.** Every project gives Jev only typed judgments (Choice/Noul/Score) over compact state; thresholds, risk vetoes, and order placement stay in deterministic code. The buberlo redesign makes this separation most explicit.
2. **Two clusters:** (a) low-latency crypto trading exploiting Jev's 70–500 ms latency (Monad/Kuru, Hyperliquid, Kraken); (b) slower financial classification/forecasting (stock direction, BANKING77, tax docs, fraud).
3. **Dry-run/paper-by-default** is the norm — real orders require explicitly configured keys.
4. No prediction-market (Polymarket), arbitrage, dedicated-forex, or DeFi-specific projects found yet (as of 2026-09-20).

## Ecosystem indexes

- [cobanov/awesome-jev](https://github.com/cobanov/awesome-jev) (★204) — most carefully source-checked list
- [everyinfra/jev-radar](https://github.com/everyinfra/jev-radar) — ecosystem tracker (220+ projects claimed)
