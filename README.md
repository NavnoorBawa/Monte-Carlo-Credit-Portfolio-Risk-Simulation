# Credit Portfolio Risk Simulation Framework

## Overview
Monte Carlo implementation of a multi-factor Merton model for credit portfolio risk assessment, calibrated with empirical financial data from authoritative sources. The model demonstrates significant tail risk underestimation (~83%) by conventional analytical approaches.

## Mathematical Framework
- **Single-Factor Model**: `Z_i = √ρ_i·Y + √(1-ρ_i)·ε_i` with default when `Z_i < Φ^(-1)(PD_i)`
- **Multi-Factor Extension**: `Z_i = Σ(a_ik·Y_k) + √(1-Σ(a_ik²))·ε_i`
- **Wrong-Way Risk Model**: `LGD_i = base_LGD_i + β_i·avg(Y) + ε_i^LGD` with β=0.1

## Data Sources
- **PD Calibration**: S&P Global Default Study (1981-2023)
- **LGD Calibration**: Moody's Recovery Rate Studies (2000-2024)
- **Correlation Parameters**: Basel II IRB formulas with empirical adjustments
- **Stress Testing**: Federal Reserve DFAST/CCAR (2021-2024)

## Installation
```bash
git clone https://github.com/username/credit-risk-simulation.git
cd credit-risk-simulation
pip install -r requirements.txt
```

## Usage
```python
from credit_risk import CreditPortfolioSimulation

# Initialize with default parameters
sim = CreditPortfolioSimulation(num_simulations=25000)

# Run base simulation
results = sim.run_simulation()

# Analytical comparison
analytical = sim.vasicek_loss_distribution()
print(f"VaR Difference: {results['Value at Risk']/analytical['Analytical VaR'] - 1:.2%}")

# Run stress tests
stress_results = sim.run_stress_scenarios()
```

## Key Results
```
Expected Loss: $20,449 (0.04% of exposure)
Value at Risk (99.9%): $830,678 (1.81% of exposure)
Expected Shortfall: $1,324,707 (2.89% of exposure)
Economic Capital: $810,229 (1.77% of exposure)
Analytical VaR: $454,771 (54.7% of simulation)
```

## Implementation Notes
- SVD decomposition for numerical stability
- Kernel-smoothed VaR estimation
- Beta distribution for stochastic LGD
- Wrong-way risk factor calibrated to crisis recovery patterns

## Validation
Model validated against:
- S&P historical default rates by rating (max deviation: 10.2% for CCC)
- Moody's industry-specific recovery rates
- Basel correlation parameters (customized with empirical adjustments)

## Dependencies
- numpy>=1.20.0
- pandas>=1.2.0
- scipy>=1.6.0
- matplotlib>=3.4.0
- seaborn>=0.11.0
- tqdm>=4.60.0

## License
MIT
