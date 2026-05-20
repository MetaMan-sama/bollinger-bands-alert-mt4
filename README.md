# Bollinger Bands Alert — MQL4 Script

A MetaTrader 4 script that monitors **Bollinger Bands** in real time and fires alerts on two distinct events: price touching or crossing the upper or lower band, and a Bollinger Band squeeze — when band width compresses below a configurable threshold.

---

## Overview

This script uses MT4's native `iBands()` function to retrieve upper, lower, and middle band values each cycle. It evaluates two independent conditions: band touch/cross events signal potential momentum breakouts, while the squeeze detector identifies periods of abnormally low volatility that often precede explosive price moves. State variables track previous band width to avoid duplicate squeeze alerts.

---

## Features

- **Band touch/cross detection** — triggers when current close meets or exceeds either band
- **Squeeze detection** — fires when band width drops below `SqueezeThreshold` for the first time
- **State tracking** — `PrevBandWidth` prevents repeat squeeze alerts on sustained compression
- **Three notification channels:** sound alert, email, and mobile push
- **Configurable symbol, timeframe, BB period, deviation, and squeeze threshold**
- **Lightweight loop** — polls once per minute (`Sleep(60000)`)
- Logs all events with price and band values to the MT4 **Experts** tab

---

## How It Works

1. Every minute, `iBands()` retrieves `MODE_UPPER`, `MODE_LOWER`, and `MODE_MAIN` band values
2. Current close is fetched via `iClose(..., 0)`
3. Two conditions are evaluated:
   - **Band Touch/Cross:**
     - `currentPrice >= upperBand` → **Price touched or crossed Upper Band**
     - `currentPrice <= lowerBand` → **Price touched or crossed Lower Band**
   - **Squeeze:** `bandWidth < SqueezeThreshold` and `PrevBandWidth >= SqueezeThreshold` → **Bollinger Bands Squeeze Detected**
4. `PrevUpperBand`, `PrevLowerBand`, and `PrevBandWidth` are updated each cycle

---

## Input Parameters

| Parameter          | Type            | Default     | Description                                         |
|--------------------|-----------------|-------------|-----------------------------------------------------|
| `TradeSymbol`      | string          | `EURUSD`    | Symbol for analysis                                 |
| `Timeframe`        | ENUM_TIMEFRAMES | `PERIOD_H1` | Timeframe for analysis                              |
| `BBPeriod`         | int             | `20`        | Bollinger Bands period                              |
| `BBDeviation`      | double          | `2.0`       | Standard deviation multiplier for band width        |
| `SqueezeThreshold` | double          | `0.0001`    | Band width below which a squeeze alert is triggered |
| `EnableAlerts`     | bool            | `true`      | Fire an on-screen/sound alert                       |
| `EnableEmail`      | bool            | `false`     | Send an email notification                          |
| `EnablePush`       | bool            | `false`     | Send a mobile push notification                     |

---

## Alert Message Format

```
Price touched or crossed Upper Band detected on EURUSD (Timeframe: PERIOD_H1)
Value: 1.08620, Band Value: 1.08615
```

---

## Installation

1. Copy `Bollinger_Bands_Alert_001.mq4` to `MQL4/Scripts/` in your MT4 data folder
2. Compile in MetaEditor (F7)
3. Drag onto any chart from Navigator → Scripts
4. Configure inputs and click **OK**

---

## Requirements

- MetaTrader 4 (`#property strict` compatible build)
- MQL4 compiler (MetaEditor)

---

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
