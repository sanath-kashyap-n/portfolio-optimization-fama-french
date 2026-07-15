# Active ETF Risk Evaluation: Fama–French 3-Factor Model

## Project Objective
This project evaluates whether actively managed equity ETFs generate genuine risk-adjusted outperformance ($\alpha$) or if their returns are simply compensation for systematic risk exposures. Using 83 monthly observations from February 2018 to December 2024, I regressed the monthly excess returns of ARK Next Generation Internet ETF (`ARKW`) against the Fama–French 3-Factor (FF3) model.

## Econometric Framework
The asset pricing model is specified as:

$$R_{it} - R_{ft} = \alpha + b_{MKT}(MKT\_RF) + b_{SMB}(SMB) + b_{HML}(HML) + \epsilon_t$$

Where:
* **$R_{it} - R_{ft}$**: ETF monthly excess return over the risk-free rate
* **$MKT\_RF$**: Market risk premium
* **$SMB$**: Size premium (Small Minus Big)
* **$HML$**: Value premium (High Minus Low)

## Statistical Results & Outputs

| Variable | Coefficient | t-stat | p-value | Significance |
| :--- | :---: | :---: | :---: | :---: |
| **Alpha ($\alpha$)** | +0.1214% | 0.20 | 0.844 | Not Significant |
| **MKT_RF ($\beta_{mkt}$)** | 1.4856% | 11.98 | 0.000 | *** (p<0.01) |
| **SMB ($\beta_{smb}$)** | 1.1364% | 5.24 | 0.000 | *** (p<0.01) |
| **HML ($\beta_{hml}$)** | -0.7121% | -4.98 | 0.000 | *** (p<0.01) |

* **Model Diagnostics:** 
  * $R^2$: **0.7633** (The 3 Fama-French factors explain ~76.3% of the active fund's return variation)
  * Annualized Alpha: **+146 bps/year** (+1.46%/year)

## Key Quantitative Interpretations
1. **Insigificant Active Premium ($\alpha$):** The annualized alpha of +146 bps is statistically indistinguishable from zero ($p = 0.844$). We cannot reject the null hypothesis; the manager failed to generate genuine risk-adjusted outperformance after controlling for style factors.
2. **Aggressive Market Exposure ($\beta_{mkt} = 1.49$):** The fund amplifies broad market movements by roughly 1.5x, confirming a highly aggressive, high-beta mandate.
3. **Small-Cap and Growth Tilt:** A positive SMB factor ($1.14$) confirms a heavy systematic allocation toward small-cap companies, while the strong negative HML factor ($-0.71$) verifies a deep growth-style tilt (holding high-valuation, low-book-to-market technology firms).

## Tech Stack
* **Language:** Python
* **Libraries:** `yfinance`, `pandas`, `numpy`, `statsmodels` (OLS regression), `matplotlib`, `pandas_datareader`
