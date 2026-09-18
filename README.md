# Active ETF Risk Evaluation: Fama–French 3-Factor Model

## Project Objective

This project evaluates whether actively managed equity ETFs generate genuine risk-adjusted outperformance ($\alpha$) or whether their returns are simply compensation for systematic risk exposures. Using 83 monthly observations from February 2018 to December 2024, I regress the monthly excess returns of the ARK Next Generation Internet ETF (`ARKW`) on the Fama–French 3-Factor (FF3) model.

## Econometric Framework

$$R_{it} - R_{ft} = \alpha + \beta_{MKT}(MKT\_RF) + \beta_{SMB}(SMB) + \beta_{HML}(HML) + \epsilon_t$$

Where:

- **$R_{it} - R_{ft}$**: ETF monthly excess return over the risk-free rate (percent per month)
- **$MKT\_RF$**: Market risk premium
- **$SMB$**: Size premium (Small Minus Big)
- **$HML$**: Value premium (High Minus Low)

Returns and factors are both expressed in percent per month, so the units are consistent. Alpha is therefore a percent-per-month figure, while the betas are unitless factor loadings.

## Statistical Results

| Variable | Coefficient | t-stat | p-value | Significance |
|---|---|---|---|---|
| **Alpha ($\alpha$)** | +0.1222 %/month | 0.20 | 0.843 | Not significant |
| **MKT_RF ($\beta_{mkt}$)** | 1.4851 | 11.96 | 0.000 | *** (p<0.01) |
| **SMB ($\beta_{smb}$)** | 1.1376 | 5.23 | 0.000 | *** (p<0.01) |
| **HML ($\beta_{hml}$)** | −0.7098 | −4.95 | 0.000 | *** (p<0.01) |

**Model diagnostics**

- $R^2$: **0.7628** — the three FF3 factors explain about 76.3% of the fund's return variation
- Annualized alpha: **+147 bps/year** (+1.47%/year), computed as the monthly intercept × 12

## Key Interpretations

1. **No significant active premium.** The annualized alpha of +147 bps is statistically indistinguishable from zero ($p = 0.843$). We cannot reject the null of zero alpha, so there is no evidence of risk-adjusted outperformance once style factors are controlled for.
2. **Aggressive market exposure ($\beta_{mkt} = 1.49$).** The fund amplifies broad market moves by roughly 1.5x, consistent with a concentrated, high-beta mandate.
3. **Small-cap and growth tilt.** The positive SMB loading (1.14) reflects a systematic tilt toward smaller companies, while the negative HML loading (−0.71) confirms a deep growth tilt — high-valuation, low book-to-market firms priced on future rather than current earnings.
4. **What the model does not explain.** The remaining ~24% of return variation is idiosyncratic: stock selection and concentrated positions the FF3 factors do not capture.

## Limitations

- Standard errors are non-robust (OLS default). Monthly factor residuals can be heteroskedastic and autocorrelated; re-estimating with HAC standard errors (`cov_type='HAC'`, `maxlags=3`) is a useful robustness check.
- The 24-month rolling regressions estimate four parameters from 24 observations, so the rolling betas are noisy and should be read as directional, not precise.
- Because Yahoo Finance re-adjusts historical prices for dividends and splits over time, re-running the notebook can shift the estimates by a few basis points. The figures above correspond to the outputs saved in the committed notebook.
- This is a single fund over a single, unusually volatile sample (2018–2024, spanning the COVID drawdown and the 2021 growth unwind). Results should not be generalized to active ETFs as a class.

## Data Sources

- **ETF prices:** Yahoo Finance via `yfinance` (auto-adjusted close)
- **Factor returns:** Ken French Data Library via `pandas_datareader` (`F-F_Research_Data_Factors`, monthly)

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook Portfolio_Optimization_Fama_French.ipynb
```

Then run all cells top to bottom. Both data sources are downloaded at runtime, so no local data files are required.

## Tech Stack

- **Language:** Python 3.11+
- **Libraries:** `yfinance`, `pandas`, `numpy`, `statsmodels` (OLS), `matplotlib`, `pandas_datareader`
