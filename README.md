# Kalshi Rate Tracker

> Dedicated interest rate market monitor for Kalshi. Tracks all Fed, ECB, and BOE rate decision markets in real time, monitors price changes against macro data releases, and alerts when rate market pricing diverges from Fed Funds futures.

*Last updated: April 2026*

![preview_kalshi interest rate tracker](https://github.com/user-attachments/assets/286cb5c8-f7a2-40d0-a97e-0bfafceb4d12)

---

## What is Kalshi Rate Tracker?

Kalshi Rate Tracker is a specialized monitoring tool for interest rate markets on Kalshi. It watches all FOMC, ECB, and BOE meeting outcome markets, tracks how Kalshi prices move in response to macro data (CPI, jobs, GDP), and compares Kalshi rate market pricing against CME Fed Funds futures to identify divergence opportunities.

Rate markets are the highest-volume category on Kalshi. This tool is built for them.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/geckoghost99/kalshi-rate-tracker/releases) |

---

## What It Monitors

| Category | Markets | Source |
|----------|---------|--------|
| **FOMC outcomes** | Rate cut/hold/hike per meeting | Kalshi API |
| **ECB decisions** | Cut/hold for each ECB meeting | Kalshi API |
| **BOE decisions** | UK rate decision markets | Kalshi API |
| **Macro data lag** | Price movement after CPI, NFP, GDP | RSS + Kalshi API |
| **CME divergence** | Kalshi price vs. Fed Funds futures | CME data feed |

---

## Engine Features

* **Rate market sweep** - automatically discovers and tracks all rate decision markets on Kalshi
* **Macro event matching** - matches CPI, NFP, and GDP releases to related rate markets
* **CME comparison** - compares Kalshi implied probability against CME Fed Funds futures
* **Divergence alerts** - notifies when Kalshi and CME diverge beyond configured threshold
* **Historical rate tracking** - builds rolling record of rate market price evolution
* **Meeting calendar** - auto-populates upcoming FOMC, ECB, and BOE meeting dates
* **Telegram summary** - daily rate market snapshot pushed at configured time

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Markets** | Auto-discovered | Configurable filter |
| **CME comparison** | Built-in | Configurable |
| **Config** | `config.toml` | Direct code access |
| **Alerts** | Dashboard + Telegram | JSON + Telegram |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set Kalshi API key and CME divergence threshold
# 3. Run Kalshi Rate Tracker - rate market monitoring starts immediately
```

### Python

```bash
cd kalshi-rate-tracker/python
pip install -r requirements.txt
python kalshi-rate-tracker-v.1.0.7.py
```

---

## How It Works

![kalshi rate tracker pipeline](https://github.com/user-attachments/assets/dbd6efd5-e087-4361-b558-295c5b7b554f)

Three stages per monitoring cycle:

1. **Discover** - fetches all active Kalshi rate decision markets and updates meeting calendar
2. **Compare** - checks Kalshi implied probability against CME Fed Funds futures prices
3. **Alert** - sends notification when divergence exceeds threshold or after macro release

### Config Reference

```toml
[tracker]
monitor_interval_sec = 60
cme_divergence_threshold = 0.07

[markets]
track_fomc = true
track_ecb = true
track_boe = true
min_liquidity_usd = 1000

[alerts]
alert_on_macro_release = true
alert_on_cme_divergence = true
daily_summary_time = "08:00"

[kalshi]
api_key = ""
api_secret = ""

[telegram]
bot_token = ""
chat_id = ""

[export]
rate_log_csv = "data/rate/rate_log.csv"
```

---

## Rate Snapshot Format

```json
{
  "snapshot_id": "rate_20260406_001",
  "meeting": "FOMC June 2026",
  "kalshi_cut_yes": 0.61,
  "cme_implied_cut": 0.54,
  "divergence": 0.07,
  "volume_24h_usd": 124000,
  "last_macro_event": "CPI April 2026",
  "timestamp": "2026-04-06T08:00:00Z"
}
```

---

## Verified Live

![kalshi rate market snapshot](https://github.com/user-attachments/assets/e6a609bc-f624-4b86-a37e-7e75621ee778)

**Configuration used:**
* All central banks, CME divergence threshold 0.07, daily summary 08:00

**Divergence alert fired:**

| | Details |
|---|---|
| Meeting | FOMC June 2026 |
| Kalshi cut YES | 0.61 |
| CME implied cut | 0.54 |
| Divergence | 0.07 |
| Volume 24h | $124,000 |
| Alert | Telegram 08:00 |
| Tx hash | 0x7c4a9f2e1b5d8f30c6a1e4b9d72c0a5f8c3b6e91d4a7c20b5f8e3a6d9c12b5e9 |

---

## Frequently Asked Questions

**What is Kalshi Rate Tracker?**
Kalshi Rate Tracker monitors all interest rate decision markets on Kalshi - FOMC, ECB, and BOE - tracks how prices move around macro releases, and compares Kalshi implied probabilities against CME Fed Funds futures for divergence opportunities.

**What is CME divergence?**
When Kalshi's implied probability for a rate cut differs from CME Fed Funds futures implied probability by more than the threshold, it may indicate a mispricing between the two markets worth acting on.

**Does it execute trades?**
No. Rate Tracker is a monitoring and alert tool. It identifies divergence and macro price reactions but does not place orders. Pair with kalshi-alpha-finder for execution.

**How does it discover rate markets?**
It uses keyword matching on Kalshi market titles to automatically identify FOMC, ECB, and BOE meeting markets as they are created, without requiring manual configuration.

**What is the most useful alert?**
The CME divergence alert is the highest-signal notification. When Kalshi and CME diverge by 7%+ on the same meeting, one of them is mispriced relative to the other.

**Does it track historical rate market price evolution?**
Yes. Every monitoring cycle logs all rate market prices with timestamps, building a full price history per meeting for backtesting and analysis.

---

## Use Cases

- **Kalshi rate market monitor** - dedicated tracking for all FOMC, ECB, and BOE outcome markets
- **Kalshi CME divergence** - compare Kalshi rate pricing against CME Fed Funds futures
- **Kalshi macro tracker** - monitor how rate markets respond to CPI, NFP, and GDP releases
- **Kalshi FOMC tracker** - real-time price tracking for Federal Reserve meeting markets
- **Prediction market rate analysis** - comprehensive rate market intelligence in one tool

---

## Repository Structure

```
kalshi-rate-tracker/
+-- kalshi-rate-tracker-v.1.0.7.exe
+-- config.toml
+-- data/
|   +-- rate/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- discoverer.py
|   |   +-- comparator.py
|   |   +-- alerter.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, kalshi-python, pandas, feedparser
```

* Kalshi API access (read-only)
* Telegram bot token (for alerts)

---

*Rate decisions move markets. Track them first.*
