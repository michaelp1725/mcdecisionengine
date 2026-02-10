# Monte Carlo Risk & Decision Engine for Trading

## Overview
A trading-focused system that evaluates whether a trade should be taken by simulating the full distribution of possible outcomes. This system focuses on decision-making under uncertainty using expected value, variance, and tail risk rather than price prediction.

## Project Structure
```
monte_carlo_trading/
├── README.md
├── requirements.txt
├── config.py                  # Configuration parameters
├── main.py                    # Main execution script
├── models/
│   ├── __init__.py
│   ├── price_models.py        # GBM, Jump Diffusion, Regime Switching
│   └── strategy_models.py     # Payoff calculations for strategies
├── simulation/
│   ├── __init__.py
│   ├── simulator.py           # Monte Carlo simulation engine
│   └── path_generator.py      # Price path generation
├── risk/
│   ├── __init__.py
│   ├── metrics.py             # VaR, CVaR, risk calculations
│   └── decision_engine.py     # Trade decision rules
├── analysis/
│   ├── __init__.py
│   ├── visualizations.py      # Plotting functions
│   └── reports.py             # Analysis and reporting
└── examples/
    ├── __init__.py
    ├── simple_long_position.py
    ├── option_strategy.py
    └── stress_testing.py
```

## Core Features

### 1. Price Path Simulation
- **Geometric Brownian Motion (GBM)**: Standard Black-Scholes dynamics
- **Jump Diffusion**: Adds discrete jump events to capture tail risks
- **Regime-Switching Volatility**: Models changing market conditions

### 2. Strategy Modeling
- Spot positions (long/short)
- Options (calls, puts, spreads, straddles)
- Transaction costs and slippage
- Position sizing analysis

### 3. Risk Metrics
- **Expected PnL**: Mean outcome across simulations
- **Variance & Standard Deviation**: Measure of dispersion
- **Downside Deviation**: Focus on negative outcomes
- **Value at Risk (VaR)**: Worst outcome at X% confidence
- **Conditional VaR (CVaR)**: Expected loss beyond VaR
- **Probability of Ruin**: Chance of catastrophic loss

### 4. Decision Engine
Trade entry criteria based on:
- Minimum expected value thresholds
- Maximum acceptable CVaR
- Risk-adjusted return requirements (Sharpe, Sortino)
- Tail risk constraints

## Installation

```bash
pip install -r requirements.txt
```

## Quick Start

```python
from simulation.simulator import MonteCarloSimulator
from models.price_models import GeometricBrownianMotion
from models.strategy_models import SpotPosition
from risk.decision_engine import DecisionEngine

# Initialize price model
gbm = GeometricBrownianMotion(
    S0=100,           # Initial price
    mu=0.05,          # Expected return (5%)
    sigma=0.20,       # Volatility (20%)
    dt=1/252          # Daily timesteps
)

# Create strategy
strategy = SpotPosition(
    position_size=1000,  # $1000 position
    entry_price=100,
    transaction_cost=0.001
)

# Run simulation
simulator = MonteCarloSimulator(
    price_model=gbm,
    strategy=strategy,
    n_simulations=10000,
    time_horizon=20  # 20 days
)

results = simulator.run()

# Make decision
engine = DecisionEngine(
    min_expected_return=0.02,
    max_cvar_95=-50,
    min_sharpe=0.5
)

decision = engine.evaluate_trade(results)
print(decision)
```

## Key Assumptions

1. **Market Microstructure**: Transaction costs are simplified; no market impact modeling
2. **Price Models**: Models are calibrated to historical data but past ≠ future
3. **Risk-Free Rate**: Assumed constant for options pricing
4. **No Leverage Constraints**: Position sizing is theoretical
5. **Model Risk**: All models are wrong; we stress test assumptions

## Evaluation Approach

This system does NOT:
- Claim to predict future prices
- Guarantee profitability
- Replace human judgment

This system DOES:
- Quantify uncertainty in trading decisions
- Reveal sensitivity to assumptions
- Highlight tail risks
- Enable systematic comparison of strategies

## Example Use Cases

### 1. Simple Long Position
Evaluate buying a stock with different volatility assumptions.

### 2. Protective Put
Analyze cost vs. downside protection trade-off.

### 3. Straddle Strategy
Model expected outcomes from volatility plays.

### 4. Stress Testing
How does the trade perform in market crashes?

## Interpretation Guidelines

- **Expected Value > 0**: Positive edge on average, but check variance
- **High CVaR**: Large tail losses possible; size accordingly
- **Low Probability of Profit**: May still be +EV if wins are large
- **Model Sensitivity**: If conclusions flip with small parameter changes, proceed cautiously

## Success Criteria

✓ Demonstrates probabilistic reasoning over point predictions  
✓ Shows trader-style risk/reward thinking  
✓ Transparent assumptions and limitations  
✓ Actionable decision framework  
✓ Robust to reasonable parameter variations  

## Further Development

- Add mean-reversion models
- Incorporate correlation structures for portfolio analysis
- Real-time data integration
- Bayesian parameter uncertainty
- Machine learning for regime detection

## Author Notes

This project is designed to mirror professional trading thought processes:
1. What's the distribution of outcomes, not the single expected outcome?
2. What's my downside in bad scenarios?
3. Am I being paid enough for the risk?
4. How robust is this to my assumptions being wrong?

**Disclaimer**: This is an educational and analytical tool. Not financial advice. Markets are uncertain and models have limitations.
