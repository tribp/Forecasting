# ARIMA

## 1. What is ARIMA?

<img src='../images/ARIMA.png' alt='Arima image' width="400px"/>

ARIMA (AutoRegressive Integrated Moving Average) is a statistical model used for time series forecasting. It combines three components:

1. AutoRegressive (AR): The model uses past values to predict future ones.
2. Integrated (I): Differencing is applied to make the time series stationary.
3. Moving Average (MA): Past forecast errors are used to refine predictions.

## ARIMA Mathematical Formulas

### **AutoRegressive (AR) Model**
Yₜ = c + φ₁ Yₜ₋₁ + φ₂ Yₜ₋₂ + ... + φₚ Yₜ₋ₚ + εₜ

The actual value is based on the sum of partial former values (also called **lagged** values). Like the temperature of today is heavily correlated with the temperature of yesterday.

### **Moving Average (MA) Model**
Yₜ = εₜ + θ₁ εₜ₋₁ + θ₂ εₜ₋₂ + ... + θ_q εₜ₋ₛ

The actual value is based on the sum of partial **errors** of former predictions. This allows to model the impact of sudden changes

### **ARMA(p, 0, q) Model**
Yₜ = c + φ₁ Yₜ₋₁ + φ₂ Yₜ₋₂ + ... + φₚ Yₜ₋ₚ + εₜ + θ₁ εₜ₋₁ + θ₂ εₜ₋₂ + ... + θ_q εₜ₋ₛ

This is the sum of the AR + MA part. The time series is stationary so no differentiation is needed. (d=0)

### **ARIMA(p, d, q) Model**
Δ^d Yₜ = φ₁ Δ^d Yₜ₋₁ + ... + φₚ Δ^d Yₜ₋ₚ + θ₁ εₜ₋₁ + ... + θ_q εₜ₋ₛ + εₜ


## 2. ARIMA versions and libraries

### 2.1 ARIMA versions

# Comparison of ARIMA, SARIMA and SARIMAX

| Model      | Components                              | Seasonal Component | Exogenous Variables | Use Case                                      |
|------------|-------------------------------------|---------|----------------------|-----------------------------------------------|
| **ARIMA**  | ARIMA | No                   | No                   | Non-seasonal time series without external influences. |
| **SARIMA** | ARIMA + Seasonality | Yes   | No                   | Time series with seasonal patterns.           |
| **SARIMAX**| ARIMA + Seasonality + Exogenous Variables | Yes  | Yes                  | Time series with seasonal patterns and external factors.(eg: holidays, DayOfWeek, planned promotions etc) |

### 2.2 ARIMA libraries

# Comparison of ARIMA Libraries in Python

| Library         | Pros                                                 | Cons                                                 |
|-----------------|------------------------------------------------------|------------------------------------------------------|
| **statsmodels** | - Well-established library.                         | - Requires more manual steps for model selection.    |
|                 | - Provides detailed statistical outputs.            | - Slower for large datasets compared to others.      |
|                 | - Handles AR, MA, ARMA, ARIMA, and SARIMA models.   | - Less automated for hyperparameter tuning.          |
| **pmdarima**    | - Easy to use with automated hyperparameter tuning. | - May not provide as detailed diagnostics as `statsmodels`. |
|                 | - Provides auto_arima function for automatic model fitting. | - Can be slower than `statsmodels` for large datasets. |

**Remark:** Notice tha although pmdarima is a wrapper around statsmodel, some functionality and syntax might slightly differ. We will be using mainly pmdarima because it offers auto_arima to automatically search for the optimal (p,d,q) combination and its ease of use.

## 3. Stationarity

**Definition:** A time series is **stationary** when all statistical parameters remain constant over time

* a constant **mean**
* a constant **variance**
* a constant **autoregression** structure
* **no seasonality** = no periodic component

A more inuitive explaination is that time series that exibit this behaviour are easier to model.

### 3.1 How can we check this?

* visualy: plot the time series and try to spot:
    * a trend
    * a varying variance

    If so then the series is **NOT Stationary**
* ADF-test: AUgmented Dicky-Fuller test

## 4 Some practical scenario's

### 4.1 AR proces

### 4.2 MA proces - example

# Cloud Cover and Solar Energy

## Given MA(1) Formula
$S_t$ = μ + $ε_t$ + θ₁ $ε_{t-1}$

where:  
- $S_t$ = Solar energy deviation on day t (as a percentage change from expected production).  
- μ = 0 (assuming we measure deviations from the norm).  
- $ε_t$ = Unexpected cloud cover impact today.  
- θ₁ = 0.5 (assuming yesterday's cloud effect still influences today).  
- $ε_{t-1}$ = Yesterday’s unexpected cloud cover impact.  

---

## Day-by-Day Calculation

### Day 1: A Sudden Thunderstorm
- $ε_1$ = -30% (unexpected severe cloud cover).  
- No previous day effect ($ε_0$ = 0).  
- **Formula:**  
  $S_1$ = 0 + (-30) + (0.5 × 0) = -30%

### Day 2: Lingering Clouds
- $ε_2$ = -10% (still some unexpected clouds).  
- Yesterday's impact: $ε_1$ = -30%.  
- **Formula:**  
  $S_2$ = 0 + (-10) + (0.5 × -30) = -10 - 15 = -25%

### Day 3: Clear Sky Returns
- $ε_3$ = 0% (no unexpected clouds today).  
- Yesterday's impact: $ε_2$ = -10%.  
- **Formula:**  
  $S_3$ = 0 + 0 + (0.5 × -10) = -5%

### Day 4: Normal Day
- $ε_4$ = 0% (perfectly normal, expected sunlight).  
- Yesterday's impact: $ε_3$ = 0%.  
- **Formula:**  
  $S_4$ = 0 + 0 + (0.5 × 0) = 0%

---

## Final Solar Energy Deviations Over Time

| Day | Unexpected Cloud Cover ($ε_t$) | Solar Output Deviation ($S_t$) |
|-----|------------------------------|------------------------------|
| 1   | -30%                         | -30%                         |
| 2   | -10%                         | -25%                         |
| 3   | 0%                           | -5%                          |
| 4   | 0%                           | 0%                           |

This example shows how **today’s solar energy deviation is influenced by today’s and yesterday’s unexpected weather conditions**—a classic **MA(1) process**.
