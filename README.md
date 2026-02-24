# Introduction to Forecasting in Julia

A comprehensive introduction to time series forecasting in Julia using the [Durbyn.jl](https://github.com/taf-society/Durbyn.jl) package. This course takes participants from Julia fundamentals through to production-ready panel data forecasting workflows.

## About Durbyn.jl

**Durbyn** -- Kurdish for "binoculars" (*Dur*, far + *Byn*, to see) -- is a native Julia package for time-series forecasting, inspired by the R **forecast** and **fable** packages. It provides a formula-based grammar for declarative model specification with full support for panel data, model comparison, and tidy forecasting workflows. Durbyn.jl is developed by the [Time Series Analysis and Forecasting Society (TAFS)](https://taf-society.org/).

## Course Structure

The training is organised into five notebooks:

| # | Notebook | Topics |
|---|----------|--------|
| 0 | [Preface](notebooks/00-preface.ipynb) | Course overview, setup instructions, notation, and references |
| 1 | [Julia Fundamentals](notebooks/01-julia-fundamentals.ipynb) | Data types, functions, control flow, multiple dispatch, type system, packages |
| 2 | [Data Wrangling with TableOps](notebooks/02-data-wrangling.ipynb) | `select`, `query`, `mutate`, `groupby`, `summarise`, `pivot_longer/wider`, joins, `PanelData` |
| 3 | [Statistical Analysis](notebooks/03-statistical-analysis.ipynb) | ACF/PACF, Box-Cox transformations, classical and STL decomposition, stationarity tests (ADF, KPSS, Phillips-Perron), differencing, Fourier terms, missing value handling |
| 4 | [Forecasting with Durbyn.jl](notebooks/04-forecasting.ipynb) | Naive methods, ETS (SES, Holt, Holt-Winters, automatic), ARIMA (manual, automatic, ARIMAX), BATS, TBATS, Theta, ARAR/ARARMA, Croston (intermittent demand), formula grammar, model collections |
| 5 | [Case Study: M3 Competition](notebooks/05-case-study.ipynb) | End-to-end panel data workflow: data preparation, EDA, multi-model specification, parallel fitting across 3,003 series, forecast generation, accuracy evaluation, model comparison |

## Prerequisites

- Graduate-level statistics (hypothesis testing, regression, maximum likelihood)
- Basic programming experience (any language)
- No prior Julia experience required -- Chapter 1 covers all essentials

## Setup

### Installing Julia

Download Julia 1.11+ from [julialang.org/downloads](https://julialang.org/downloads/).

### Installing Durbyn.jl

```julia
using Pkg
Pkg.add(url="https://github.com/taf-society/Durbyn.jl")
```

### Additional Packages

```julia
using Pkg
Pkg.add(["CSV", "Downloads", "Plots"])
```

### Using the Workshop Environment (Recommended)

A reproducible project environment is provided in `notebooks/workshop/`:

```julia
using Pkg
Pkg.activate("notebooks/workshop")
Pkg.instantiate()
```

### Multi-Threading for Performance

Durbyn supports automatic parallel computing for panel data. Start Julia with multiple threads for significant speedups:

```bash
julia -t auto  # Use all available CPU cores
```

Performance scales with cores: 8 cores yield roughly 8x speedup, 32 cores approximately 20x.

## Dataset

The training uses the **M3 Competition** dataset (Makridakis & Hibon, 2000), sourced from the [Mcomp](https://cran.r-project.org/web/packages/Mcomp/) R package (Hyndman et al., GPL-3). It contains 3,003 real-world time series at yearly, quarterly, and monthly frequencies across six domains:

| Domain | Yearly | Quarterly | Monthly | Total |
|--------|--------|-----------|---------|-------|
| Micro | 146 | 204 | 474 | 824 |
| Macro | 83 | 99 | 312 | 494 |
| Industry | 102 | 83 | 334 | 519 |
| Finance | 58 | 76 | 145 | 279 |
| Demographic | 245 | 57 | 111 | 413 |
| Other | 11 | 37 | 52 | 100 |
| **Total** | **645** | **556** | **1,428** | **3,003** |

Each series is split into a training portion (`x`) and a hold-out test portion (`xx`), with standardised test horizons (Yearly: 6, Quarterly: 8, Monthly: 18).

Data files are located in `data/`:
- `M3_MONTHLY.csv`
- `M3_QUARTERLY.csv`
- `M3_YEARLY.csv`

## Models Covered

| Model Family | Methods |
|---|---|
| **Benchmarks** | Naive, Seasonal Naive, Random Walk (with drift), Mean |
| **Exponential Smoothing** | SES, Holt's Linear, Holt-Winters (additive/multiplicative), Automatic ETS (30 model variants) |
| **ARIMA** | Manual ARIMA/SARIMA, Automatic ARIMA, ARIMAX (with regressors) |
| **BATS / TBATS** | Box-Cox ARMA Trend Seasonal, Trigonometric BATS |
| **Theta** | STM, OTM, DSTM, DOTM (Dynamic Optimised Theta) |
| **ARAR / ARARMA** | Autoregressive AR, Auto ARARMA |
| **Intermittent Demand** | Croston (Classic, SBA, SBJ) |

## Repository Structure

```
.
├── README.md
├── data/
│   ├── M3_MONTHLY.csv
│   ├── M3_QUARTERLY.csv
│   └── M3_YEARLY.csv
└── notebooks/
    ├── 00-preface.ipynb
    ├── 01-julia-fundamentals.ipynb
    ├── 02-data-wrangling.ipynb
    ├── 03-statistical-analysis.ipynb
    ├── 04-forecasting.ipynb
    ├── 05-case-study.ipynb
    └── workshop/
        └── Project.toml
```

## References

### General Forecasting

- Hyndman, R. J., & Athanasopoulos, G. (2021). *Forecasting: Principles and Practice* (3rd ed.). OTexts. https://otexts.com/fpp3/

### Exponential Smoothing (ETS)

- Hyndman, R. J., Koehler, A. B., Snyder, R. D., & Grose, S. (2002). A State Space Framework for Automatic Forecasting Using Exponential Smoothing Methods. *International Journal of Forecasting*, *18*(3), 439--454.
- Hyndman, R. J., Akram, Md., & Archibald, B. (2008). The Admissible Parameter Space for Exponential Smoothing Models. *Annals of the Institute of Statistical Mathematics*, *60*(2), 407--426.
- Hyndman, R. J., Koehler, A. B., Ord, J. K., & Snyder, R. D. (2008). *Forecasting with Exponential Smoothing: The State Space Approach*. Springer.

### ARIMA

- Box, G. E. P., Jenkins, G. M., & Reinsel, G. C. (2015). *Time Series Analysis: Forecasting and Control* (5th ed.). Wiley.
- Hamilton, J. D. (1994). *Time Series Analysis*. Princeton University Press.
- Hyndman, R. J., & Khandakar, Y. (2008). Automatic Time Series Forecasting: The forecast Package for R. *Journal of Statistical Software*, *27*(3).
- Kunst, R. (2011). Applied Time Series Analysis -- Part II. University of Vienna.

### BATS / TBATS

- De Livera, A. M., Hyndman, R. J., & Snyder, R. D. (2011). Forecasting Time Series with Complex Seasonal Patterns Using Exponential Smoothing. *Journal of the American Statistical Association*, *106*(496), 1513--1527.

### Theta Method

- Assimakopoulos, V., & Nikolopoulos, K. (2000). The Theta Model: A Decomposition Approach to Forecasting. *International Journal of Forecasting*, *16*(4), 521--530.
- Fiorucci, J. A., Pellegrini, T. R., Louzada, F., Petropoulos, F., & Koehler, A. B. (2016). Models for Optimising the Theta Method and Their Relationship to State Space Models. *International Journal of Forecasting*, *32*(4), 1151--1161.

### Intermittent Demand (Croston)

- Croston, J. D. (1972). Forecasting and Stock Control for Intermittent Demands. *Operational Research Quarterly*, *23*(3), 289--303.
- Syntetos, A. A., & Boylan, J. E. (2005). The Accuracy of Intermittent Demand Estimates. *International Journal of Forecasting*, *21*(2), 303--314.
- Shenstone, L., & Hyndman, R. J. (2005). Stochastic Models Underlying Croston's Method for Intermittent Demand Forecasting. *Journal of Forecasting*, *24*, 389--402.
- Kourentzes, N. (2014). On Intermittent Demand Model Optimisation and Selection. *International Journal of Production Economics*, *156*, 180--190.

### ARAR / ARARMA

- Brockwell, P. J., & Davis, R. A. (2002). *Introduction to Time Series and Forecasting* (2nd ed.). Springer.
- Parzen, E. (1982). ARARMA Models for Time Series Analysis and Forecasting. *Journal of Forecasting*, *1*(1), 67--82.

### Box-Cox Transformation

- Box, G. E. P., & Cox, D. R. (1964). An Analysis of Transformations. *Journal of the Royal Statistical Society: Series B*, *26*(2), 211--246.
- Guerrero, V. M. (1993). Time-Series Analysis Supported by Power Transformations. *Journal of Forecasting*, *12*, 37--48.

### Decomposition

- Cleveland, R. B., Cleveland, W. S., McRae, J. E., & Terpenning, I. (1990). STL: A Seasonal-Trend Decomposition Procedure Based on Loess. *Journal of Official Statistics*, *6*(1), 3--73.

### Unit Root and Stationarity Tests

- Dickey, D. A., & Fuller, W. A. (1979). Distribution of the Estimators for Autoregressive Time Series with a Unit Root. *Journal of the American Statistical Association*, *74*(366a), 427--431.
- Said, S. E., & Dickey, D. A. (1984). Testing for Unit Roots in ARMA Models of Unknown Order. *Biometrika*, *71*, 599--607.
- Kwiatkowski, D., Phillips, P. C. B., Schmidt, P., & Shin, Y. (1992). Testing the Null Hypothesis of Stationarity Against the Alternative of a Unit Root. *Journal of Econometrics*, *54*(1--3), 159--178.
- Phillips, P. C. B., & Perron, P. (1988). Testing for a Unit Root in Time Series Regression. *Biometrika*, *75*(2), 335--346.
- MacKinnon, J. G. (1991). Critical Values for Cointegration Tests. In R. F. Engle & C. W. J. Granger (Eds.), *Long-Run Economic Relationships*. Oxford University Press.

### Seasonal Integration Tests

- Osborn, D. R., Chui, A. P. L., Smith, J., & Birchenhall, C. R. (1988). Seasonality and the Order of Integration for Consumption. *Oxford Bulletin of Economics and Statistics*, *50*(4), 361--377.
- Osborn, D. R. (1990). A Survey of Seasonality in UK Macroeconomic Variables. *International Journal of Forecasting*, *6*, 327--336.
- Wang, X., Smith, K. A., & Hyndman, R. J. (2006). Characteristic-Based Clustering for Time Series Data. *Data Mining and Knowledge Discovery*, *13*(3), 335--364.

### Diffusion Models

- Bass, F. M. (1969). A New Product Growth Model for Consumer Durables. *Management Science*, *15*(5), 215--227.
- Rogers, E. M. (1962). *Diffusion of Innovations*. Free Press.
- Gompertz, B. (1825). On the Nature of the Function Expressive of the Law of Human Mortality. *Philosophical Transactions of the Royal Society*, *115*, 513--583.
- Bemmaor, A. C. (1994). Modeling the Diffusion of New Durable Goods. In G. Laurent et al. (Eds.), Kluwer.
- Jukic, D., Kralik, G., & Scitovski, R. (2004). Least-Squares Fitting Gompertz Curve. *Journal of Computational and Applied Mathematics*, *169*, 359--375.
- Sharif, M. N., & Islam, M. N. (1980). The Weibull Distribution as a General Model for Forecasting Technological Change. *Technological Forecasting and Social Change*, *18*(3), 247--256.

### Datasets

- Makridakis, S., & Hibon, M. (2000). The M3-Competition: Results, Conclusions and Implications. *International Journal of Forecasting*, *16*(4), 451--476.
- Campbell, M. J., & Walker, A. M. (1977). A Survey of Statistical Work on the Mackenzie River Series of Annual Canadian Lynx Trappings. *Journal of the Royal Statistical Society: Series A*, *140*, 411--431.
- Andrews, D. F., & Herzberg, A. M. (1985). *Data: A Collection of Problems from Many Fields*. Springer.

## License

This work is licensed under a [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share and adapt this material for any purpose, provided you give appropriate attribution.

### Dataset Notice

The M3 Competition data included in `data/` originates from the [Mcomp](https://cran.r-project.org/web/packages/Mcomp/) R package by Rob Hyndman, Muhammad Akram, Christoph Bergmeir, and Mitchell O'Hara-Wild, licensed under [GPL-3](https://www.gnu.org/licenses/gpl-3.0.html). The original dataset is described in:

> Makridakis, S., & Hibon, M. (2000). The M3-Competition: Results, Conclusions and Implications. *International Journal of Forecasting*, *16*(4), 451--476.
