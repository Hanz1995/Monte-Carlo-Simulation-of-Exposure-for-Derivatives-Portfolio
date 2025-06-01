# Monte Carlo Simulation of Exposure for Derivatives Portfolio

This project implements a Monte Carlo simulation framework to evaluate **Counterparty Credit Risk (CCR)** for a derivatives portfolio consisting of:

- A fixed-for-floating **Interest Rate Swap (IRS)** referencing 6M LIBOR
- A **Foreign Exchange Forward (FX Forward)** on the USD/EUR pair

The simulation computes:
- **Expected Exposure (EE)**
- **Potential Future Exposure (PFE)** at 97.5th percentile
- **Exposure at Default (EAD)**
- **Risk-Weighted Assets (RWA)** under the **Basel III** standardized approach

The goal is to provide a transparent and reproducible framework for evaluating CCR metrics in an **uncollateralized** setting using simplified assumptions and open data.

---

##  Project Objectives

1. **Simulate market risk factors** (interest rates and FX rates) via Geometric Brownian Motion
2. **Construct a two-product derivatives portfolio**
3. **Compute MtM values** for each product along each simulated path
4. **Aggregate exposures** to generate EE and PFE curves
5. **Estimate regulatory capital** using Basel III rules

Simulations are performed on a **monthly basis**, with each product modeled over its contractual horizon. The PFE is set at the **97.5th percentile**, consistent with regulatory expectations for stress-based exposure.

---

##  Methodology

### Risk Factor Simulation
- **6M LIBOR** and **USD/EUR FX** paths simulated using Geometric Brownian Motion (GBM)
- Assumes monthly steps for up to 1 or 2 years, with 1,000 paths
- Drift and volatility parameters are user-defined (e.g., 10% annualized volatility)

### Exposure Calculation
- **IRS** and **FX Forward** exposures computed from MtM values under a fixed-vs-floating structure
- Exposure = `max(MtM, 0)` at each time step and path
- EE and PFE are aggregated across paths for each future date

### Regulatory Metrics
- **EAD** = $\( \alpha \cdot \max(\text{EE}) \), with \( \alpha = 1.4 \)$
- **RWA** = EAD × standardized risk weight (20%)
- The exposure curve reflects the time-varying nature of market-dependent counterparty risk

---

##  Visual Outputs

The project includes the following charts:

- Simulated **FX paths** (USD/EUR)
- Simulated **LIBOR paths** (6M tenor)
- MtM distribution for each product (IRS, FX Forward)
- **Exposure curve**: Expected Exposure (EE) and Potential Future Exposure (PFE)

> All visualizations are generated using matplotlib and located in the **Appendix** section of the notebook. Function definitions are preserved but not explained in detail.

---

##  Capital Summary by Product

| Metric         | USD (million) |
|----------------|----------------|
| EAD (IRS)      | 296.19         |
| EAD (FX)       | 31,526.53      |
| **EAD (Total)**| **31,822.72**  |
| RWA (IRS)      | 59.24          |
| RWA (FX)       | 6,305.31       |
| **RWA (Total)**| **6,364.54**   |

These results reflect:
- Independent simulation and exposure calculation per product
- No netting benefit assumed across instruments (uncollateralized basis)
- Additivity of EAD and RWA under standardized approach

This decomposition helps assess **product-level capital consumption** and supports granular **risk-based decision making**.


---

## References

- [Basel III: Finalising post-crisis reforms](https://www.bis.org/bcbs/publ/d424.htm)
- [SR 11-7: Federal Reserve Guidance on Model Risk Management](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm)

---

## Disclaimer

This is an independent academic project for illustrative purposes only. The models and assumptions used herein are simplified and do not reflect the complexities of production-grade risk engines.

---

## License

This project is licensed under the [MIT License](./LICENSE).

