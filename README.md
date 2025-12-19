# Volatility Smile

[Interactive Volatility Smile]([https://github.com/mike522-byte/finance-analytic/blob/main/volatility_smile.html](https://htmlpreview.github.io/?https://raw.githubusercontent.com/mike522-byte/finance-analytic/refs/heads/main/volatility_smile.html))

# Event trading - Shorting Vega 

## 1. Strategy Overview

### 1.1 Core Trading Opportunity
Sell options before a major event (earnings, central bank decisions, etc.), betting on a sharp drop in implied volatility (IV) post-event (volatility crush), while managing the gamma risk caused by the event.

### 1.2 Logic Chain

Market Expectation → Implied Jump → Risk Pricing → Trading Decision

Front-month IV rises → Calculate J → Assess Gamma Risk → Decide to Short


## 2. Theoretical Basis

### 2.1 Market Phenomenon Observations
1. **Pre-event**: Uncertainty drives up near-month IV, forming a "volatility spike."
2. **Post-event**: Uncertainty resolves, IV "crushes" back to normal levels.
- Key Assumption: The event is the dominant factor causing the difference between front-month and second-month IV.

### 2.2 Calculate Forward Volatility and Jump Magnitude
Assume two expiration dates:
- $T_1$: Front-month expiration (covers the event)
- $T_2$: Second-month expiration
- $\sigma_1$: Front-month ATM IV
- $\sigma_2$: Second-month ATM IV

When $\sigma_1 > \sigma_2$, calculate the forward volatility from $T_1$ to $T_2$:

$$
\sigma_{12} = \sqrt{\frac{\sigma_2^2 T_2 - \sigma_1^2 T_1}{T_2 - T_1}} 
$$

The instantaneous price jump percentage $J$ implied by the event:

$$
J \approx \sqrt{\sigma_1^2 T_1 - \sigma_{12}^2 T_1}
$$

Indicates the additional volatility premium the market prices for the event day.

## 4. Calculation Steps Example

### Input Data
| Parameter | Example Value |
|--------|---------------|
| $T_1$ | 0.0384 years (14 days) |
| $T_2$ | 0.1151 years (42 days) |
| $\sigma_1$ | 45% |
| $\sigma_2$ | 30% | 

### Calculation Process
1. Calculate Forward Volatility:

$$
\sigma_{12} = \sqrt{\frac{0.30^2 \times 0.1151 - 0.45^2 \times 0.0384}{0.1151 - 0.0384}} = 25.6\%
$$

3. Calculate Implied Jump:
   
$$
J = \sqrt{0.45^2 \times 0.0384 - 0.256^2 \times 0.0384} = 8.7\%
$$

The front-month IV contains an event risk premium of **19.4%** (45% - 25.6%)

## 5. Trading Decision Framework

### Risk Assessment Matrix
| Actual Move vs Implied Jump | Vega Profit | Gamma Risk | Strategy Performance |
|-----------------------------|-------------|------------|----------------------|
| Actual Move < J | Gain from IV drop | Limited loss | **Profitable** |
| Actual Move ≈ J | Gain from IV drop | Loss offsets gain | Break-even |
| Actual Move > J | Gain from IV drop | Loss exceeds gain | **Loss** |

### Key Considerations
1. **Time Decay Advantage**: Benefits from positive Theta.
2. **IV Term Structure**: Must ensure $\sigma_1 > \sigma_2$ with significant difference.
3. **Liquidity Requirement**: Need to trade in **active underlyings** to control bid-ask spread costs.

---

**Strategy Core**: By quantifying the market's pricing of event risk (the $J$-value), short the overpriced event volatility premium in a risk-controlled manner, earning Vega profit from the IV crush, while enhancing returns through time decay (Theta).
