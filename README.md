# Volatility Inventory Market Making Strategy

This strategy implements a **market making bot** for ETH-USDT perpetual futures on Binance Testnet. It dynamically places buy and sell limit orders based on recent market volatility (NATR) and momentum (RSI). Inventory risk is managed by adjusting quotes based on the current portfolio composition.

## **Indicators Used**

* **NATR (Normalized Average True Range):**
  Measures recent price volatility. Used to set the width of bid/ask spreads.
* **RSI (Relative Strength Index):**
  Measures market momentum. Used to shift quote prices away from the mid-price if overbought or oversold conditions are detected.

## **Key Parameters**

| Parameter             | Value    | Description                     |
| --------------------- | -------- | ------------------------------- |
| Trading Pair          | ETH-USDT | Binance Perpetual Testnet       |
| NATR Period           | 30       | Volatility lookback (candles)   |
| RSI Period            | 30       | RSI lookback (candles)          |
| Bid Spread Multiplier | 120      | Controls bid width (basis NATR) |
| Ask Spread Multiplier | 60       | Controls ask width (basis NATR) |
| Order Refresh Time    | 15 sec   | Re-quotes every X seconds       |
| Order Size            | 0.01 ETH | Size per order                  |
| Target Base Ratio     | 0.5      | Target 50% allocation in ETH    |

## **How it Works**

1. **Fetches recent OHLCV candles (1m) using ccxt.**
2. **Calculates mid price, NATR, and RSI.**
3. **Dynamically computes bid/ask spreads based on NATR.**
4. **Shifts order prices based on:**

   * **RSI:** More aggressive if overbought/oversold.
   * **Inventory:** Quotes skewed to reduce risk if holding excess ETH or USDT.
5. **Places/cancels limit orders on both sides of the book.**
6. **Positions and balances are monitored and displayed for live strategy tracking.**

## **Risk Controls**

* **Inventory management:** Order prices adapt to current portfolio, targeting neutral exposure.
* **Order cancellation:** Old orders are canceled before new ones are placed.
* **Frequent re-quoting:** Spreads adapt to new volatility, minimizing adverse selection.

## **Key metrics to monitor:**

  * Filled orders and realized PnL
  * Live inventory and position size
  * Spread capture and execution quality

## **Visualization**

Live dashboard includes:

* Current balances and active orders
* Position size and entry price
* Bid/ask spreads, latest RSI/NATR
* Recent candle data for transparency
