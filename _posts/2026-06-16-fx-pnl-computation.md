---
layout: post
title: "FX PNL Computation"
date: 2026-06-16 04:38:00 +0400
categories: finance
---

## Motivation & Core Caveats of FX PnL

Unlike equities where a trade typically involves buying/selling a stock settled in a single base currency (e.g., buying Apple in USD), trading Foreign Exchange (FX) presents unique structural complexities. 

1. **Dual Currency Transactions**: Every spot FX trade is a simultaneous transaction: buying one currency (the base currency) and selling another (the quote currency). 
2. **Financing and Carry Costs ($$r$$)**: Spot trades typically settle on a $$T+2$$ basis. If a position is held overnight, it must be rolled over (typically via a Tom-Next swap). This incurs or earns interest based on the differential between the interest rates of the two currencies involved. Ignoring these interest rates over longer horizons leads to inaccurate PnL calculations.
3. **Quote Currency vs. Reporting Currency Conversion**: 
   - When trading a pair like **EUR/USD** (with USD as the quote currency) for a USD-denominated account, the PnL is naturally denominated in USD.
   - However, when trading a pair like **USD/JPY** (where the quote currency JPY differs from the reporting currency USD), the JPY-denominated PnL must be converted back to USD at the exit spot rate. Because the conversion rate itself varies between the entry time $$T$$ and exit time $$T+1$$, this creates a non-linear relationship and introduces currency risk on the PnL itself.
4. **Fast Trading Approximation**: For high-frequency or intra-day trading, positions are closed before the overnight roll. Under these conditions, we can approximate the financing rates $$r \to 0$$, simplifying the formulas to the standard "Return on Notional" approximations.

This page covers the exact math and the corresponding fast-trading approximations for both standard base-reporting (EUR/USD) and quote-reporting (USD/JPY) currency pairs.

---

## EUR/USD Spot Trade

### Case 1: Long EUR, Short USD
**Spot Transaction:** Buy $$1 \text{ EUR} \leftarrow \text{Pay } \text{Spot}_T \text{ USD}$$

Initial state at $$T$$:

$$+1 \text{ EUR} - \text{Spot}_{T} \text{ USD} = 0 \text{ USD}$$

Value at $$T+1$$ (including interest):

$$(1 + r_{\text{EUR}}) \cdot \text{Spot}_{T+1} \text{ USD} - \text{Spot}_{T}(1 + r_{\text{USD}}) \text{ USD}$$

Rearranged for Exact PNL:

$$\text{PNL} = (\text{Spot}_{T+1} - \text{Spot}_{T})\text{ USD} + (r_{\text{EUR}} \cdot \text{Spot}_{T+1} - r_{\text{USD}} \cdot \text{Spot}_{T})\text{ USD}$$

**Fast Trading Approximation ($$r \to 0$$):**

$$\lim_{r \to 0} \text{PNL} = (\text{Spot}_{T+1} - \text{Spot}_{T}) \text{ USD}$$

$$\text{Return on Notional} = \frac{\text{PNL in USD}}{\text{Initial USD Exposure}} = \frac{\text{Spot}_{T+1} - \text{Spot}_{T}}{\text{Spot}_{T}}$$

---

### Case 2: Short EUR, Long USD
**Spot Transaction:** Sell $$1 \text{ EUR} \rightarrow \text{Receive } \text{Spot}_T \text{ USD}$$

Initial state at $$T$$:

$$-1 \text{ EUR} + \text{Spot}_{T} \text{ USD} = 0 \text{ USD}$$

Value at $$T+1$$ (including interest):

$$-(1 + r_{\text{EUR}})\text{Spot}_{T+1} \text{ USD} + \text{Spot}_{T}(1 + r_{\text{USD}}) \text{ USD}$$

Rearranged for Exact PNL:

$$\text{PNL} = -(\text{Spot}_{T+1} - \text{Spot}_{T})\text{ USD} - (r_{\text{EUR}} \cdot \text{Spot}_{T+1} - r_{\text{USD}} \cdot \text{Spot}_{T})\text{ USD}$$

**Fast Trading Approximation ($$r \to 0$$):**

$$\lim_{r \to 0} \text{PNL} = -(\text{Spot}_{T+1} - \text{Spot}_{T}) \text{ USD}$$

$$\text{Return on Notional} = \frac{\text{PNL in USD}}{\text{Initial USD Exposure}} = \frac{-(\text{Spot}_{T+1} - \text{Spot}_{T})}{\text{Spot}_{T}}$$

---

## USD/JPY Spot Trade

### Case 1: Long USD, Short JPY
**Spot Transaction:** Buy $$1 \text{ USD} \leftarrow \text{Pay } \text{Spot}_T \text{ JPY}$$

Initial state at $$T$$:

$$+1 \text{ USD} - \text{Spot}_{T} \text{ JPY} = 0 \text{ USD}$$

Value at $$T+1$$ converted back to USD (Quote Currency $$\to$$ Base Currency):

$$\text{Exact PNL} = (1 + r_{\text{USD}})\text{ USD} - \frac{\text{Spot}_{T}}{\text{Spot}_{T+1}}(1 + r_{\text{JPY}})\text{ USD}$$

**Fast Trading Approximation ($$r \to 0$$):**

$$\lim_{r \to 0} \text{PNL (in USD)} = \left( 1 - \frac{\text{Spot}_{T}}{\text{Spot}_{T+1}} \right) \text{ USD} = \left( \frac{\text{Spot}_{T+1} - \text{Spot}_{T}}{\text{Spot}_{T+1}} \right) \text{ USD}$$

$$\text{Return on Notional} = \frac{\text{PNL in USD}}{\text{Initial USD Exposure (1 USD)}} = \frac{\text{Spot}_{T+1} - \text{Spot}_{T}}{\text{Spot}_{T+1}}$$

---

### Case 2: Short USD, Long JPY
**Spot Transaction:** Sell $$1 \text{ USD} \rightarrow \text{Receive } \text{Spot}_T \text{ JPY}$$

Initial state at $$T$$:

$$-1 \text{ USD} + \text{Spot}_{T} \text{ JPY} = 0 \text{ USD}$$

Value at $$T+1$$ converted back to USD:

$$\text{Exact PNL} = -(1 + r_{\text{USD}})\text{ USD} + \frac{\text{Spot}_{T}}{\text{Spot}_{T+1}}(1 + r_{\text{JPY}})\text{ USD}$$

**Fast Trading Approximation ($$r \to 0$$):**

$$\lim_{r \to 0} \text{PNL (in USD)} = \left( -1 + \frac{\text{Spot}_{T}}{\text{Spot}_{T+1}} \right) \text{ USD} = -\left( \frac{\text{Spot}_{T+1} - \text{Spot}_{T}}{\text{Spot}_{T+1}} \right) \text{ USD}$$

$$\text{Return on Notional} = \frac{\text{PNL in USD}}{\text{Initial USD Exposure (1 USD)}} = -\left( \frac{\text{Spot}_{T+1} - \text{Spot}_{T}}{\text{Spot}_{T+1}} \right)$$
