# Advanced Trading Strategy

## Overview
This project implements an advanced trading strategy utilizing arbitrage, market making, and risk management techniques. The system is designed to trade cryptocurrencies like ETH, BTC, and LTC while ensuring capital allocation, managing risk, and using statistical price predictions.

The code is composed of multiple classes that handle order placement, price prediction, and trading execution. The trading strategies include **Market Making** and **Arbitrage** trading, driven by real-time market data.

## Key Components

### Configuration Parameters
- **Starting Capital:** Initial capital for the strategy (`STARTING_CAPITAL`).
- **Risk Management:**
  - **Position Limit:** Max percentage of capital in one asset (`RISK_POSITION_LIMIT_PCT`).
  - **Stop Loss:** Stop loss percentage (`STOP_LOSS_PCT`).
  - **Trailing Stop Loss:** Trailing stop loss percentage (`TRAILING_STOP_LOSS_PCT`).
- **Market Making Strategy:**
  - Parameters controlling spreads, volatility, and order quantity (`MM_*` parameters).
- **Arbitrage Strategy:**
  - Capital allocation percentage for arbitrage trades (`ARB_CAPITAL_ALLOCATION_PCT`).

### Key Classes and Methods

#### `Strategy` Class
Handles overall trading operations, including market making, arbitrage, and risk management.

1. **on_orderbook_update**: Updates the local order book and computes the fair price using the `Prediction` class. This method is called whenever the order book is updated.
2. **on_trade_update**: Updates price history based on trade updates.
3. **on_account_update**: Updates the capital and positions after a trade is executed.
4. **execute_trading_strategy**: Executes the trading strategy by calling the `market_making_strategy`, `arbitrage_strategy`, and risk management checks.
5. **arbitrage_strategy**: Executes arbitrage trading based on discrepancies between fair price and market prices.
6. **market_making_strategy**: Places limit orders on both the buy and sell sides, ensuring the spread is maintained.
7. **check_risk_management**: Enforces position limits and stop-losses to mitigate risk.

#### `Prediction` Class
This class predicts the fair price using a **Sliding Window Median** algorithm and combines it with a **Kernel Density Estimator (KDE)** to produce a more accurate price prediction.

1. **update**: Updates bids, asks, and the price book based on incoming order book data.
2. **predict**: Returns the combined prediction using the soft average and KDE-based estimation.
3. **update_kde**: Updates the KDE model based on price history.

### Arbitrage Strategy
The arbitrage strategy exploits price differences between the fair price (calculated by the predictor) and the market prices. The system automatically buys if the market price is lower than the fair price and sells if it is higher.

### Market Making Strategy
Market making involves placing both buy and sell orders around the predicted fair price, profiting from the spread between the bid and ask prices. The system dynamically adjusts the spread based on market volatility and ensures that the position limits are maintained.

### Risk Management
Risk management is handled through:
- **Position limits**, preventing any single asset from exceeding a percentage of the total capital.
- **Stop-loss** and **trailing stop-loss** mechanisms to minimize losses during adverse market conditions.

### Order Placement
The strategy supports:
- **Market orders**: Immediate buy/sell at the current market price.
- **Limit orders**: Buy/sell orders at a specified price, used mainly in the market making strategy.

## How It Works

1. **Price Prediction**: The `Prediction` class continuously updates the fair price of an asset by analyzing the order book and historical prices.
2. **Order Book Updates**: As the market changes, the strategy updates its view of the order book and adjusts its predictions.
3. **Strategy Execution**: The trading strategy (market making or arbitrage) is executed based on the current fair price and order book data.
4. **Risk Management**: The system continuously monitors position sizes and enforces stop-loss rules to protect capital.
