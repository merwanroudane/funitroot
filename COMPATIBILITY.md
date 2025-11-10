# GAUSS Code Compatibility

## Overview

This Python implementation of Fourier ADF and Fourier KPSS tests is **fully compatible** with the original GAUSS implementations by Saban Nazlioglu.

## Verified Compatibility

### ✅ Fourier ADF Test

**Original GAUSS Code:** `fourier_adf` by Saban Nazlioglu  
**Reference:** Enders, W., and Lee, J. (2012)

#### Critical Values
- ✅ Sample-size dependent critical values (T ≤ 150, 151-349, 350-500, >500)
- ✅ Model-specific critical values (constant vs. constant+trend)
- ✅ Frequency-specific critical values (k=1 to 5)
- ✅ Exact match with GAUSS `_getFourierADFCrit` function

#### Test Statistics
- ✅ Test statistic calculation matches GAUSS `_runFourierOLS`
- ✅ Lag selection via AIC, BIC, or t-stat significance
- ✅ Frequency selection via minimum test statistic

#### Fourier Terms
- ✅ Trigonometric terms: `sin(2πkt/T)` and `cos(2πkt/T)`
- ✅ Matches GAUSS `_getFourierTerms` function

### ✅ Fourier KPSS Test

**Original GAUSS Code:** `fourier_kpss` by Saban Nazlioglu  
**Reference:** Becker, R., Enders, W., and Lee, J. (2006)

#### Critical Values
- ✅ Sample-size dependent critical values (T ≤ 250, 251-500, >500)
- ✅ Model-specific critical values (level vs. trend stationarity)
- ✅ Frequency-specific critical values (k=1 to 5)
- ✅ Exact match with GAUSS `_getFourierKPSSCrit` function

#### Frequency Selection
- ✅ Uses minimum SSR (Sum of Squared Residuals) method
- ✅ Matches GAUSS `_get_fourier` function exactly
- ⚠️ **Important:** Unlike some implementations that use minimum test statistic, this uses minimum SSR as in the original GAUSS code

#### Long-Run Variance
- ✅ Newey-West HAC estimator
- ✅ Bartlett kernel weighting
- ✅ Automatic lag selection: `round(4 * (T/100)^(2/9))`

## Critical Value Tables

### Fourier ADF Critical Values (Model: Constant)

| Sample Size | k=1 CV (5%) | k=2 CV (5%) | k=3 CV (5%) | k=4 CV (5%) | k=5 CV (5%) |
|-------------|-------------|-------------|-------------|-------------|-------------|
| T ≤ 150     | -3.81       | -3.27       | -3.07       | -2.97       | -2.93       |
| 151-349     | -3.78       | -3.26       | -3.06       | -2.98       | -2.94       |
| 350-500     | -3.76       | -3.26       | -3.06       | -2.97       | -2.94       |
| T > 500     | -3.75       | -3.25       | -3.05       | -2.96       | -2.93       |

### Fourier KPSS Critical Values (Model: Constant)

| Sample Size | k=1 CV (5%) | k=2 CV (5%) | k=3 CV (5%) | k=4 CV (5%) | k=5 CV (5%) |
|-------------|-------------|-------------|-------------|-------------|-------------|
| T ≤ 250     | 0.1720      | 0.4152      | 0.4480      | 0.4592      | 0.4626      |
| 251-500     | 0.1696      | 0.4075      | 0.4424      | 0.4491      | 0.4571      |
| T > 500     | 0.1704      | 0.4047      | 0.4388      | 0.4470      | 0.4525      |

## Key Implementation Details

### 1. Frequency Selection

**Fourier ADF:**
```python
# Select frequency with minimum (most negative) test statistic
f = minindc(tauk)  # GAUSS
opt_freq = np.argmin(statistics)  # Python
```

**Fourier KPSS:**
```python
# Select frequency with minimum SSR
f = keep_k[minindc(keep_ssr)]  # GAUSS
opt_freq = frequencies[np.argmin(ssr_values)]  # Python
```

### 2. Fourier Terms Generation

Both implementations use:
```
sin_term = sin(2 * π * k * t / T)
cos_term = cos(2 * π * k * t / T)
```

where:
- k = frequency (1 to 5)
- t = time index (1 to T)
- T = sample size

### 3. Model Specifications

**Model 1 (Constant):**
- ADF: `Δy_t = α + δy_{t-1} + sin + cos + lags`
- KPSS: Detrend with `y_t = α + sin + cos`

**Model 2 (Constant + Trend):**
- ADF: `Δy_t = α + βt + δy_{t-1} + sin + cos + lags`
- KPSS: Detrend with `y_t = α + βt + sin + cos`

## Differences from Other Implementations

### This Implementation (Python - GAUSS Compatible)
✅ Sample-size dependent critical values  
✅ KPSS uses minimum SSR for frequency selection  
✅ Exact critical value tables from original papers  

### Some Other Implementations
❌ Fixed critical values (not sample-size dependent)  
❌ KPSS uses minimum statistic (not minimum SSR)  
❌ Simplified critical value tables  

## Validation

### Test Results Match GAUSS:
- ✅ Critical values identical for all sample sizes
- ✅ Frequency selection method identical
- ✅ Test statistic calculations identical
- ✅ Model specifications identical

### Verified With:
- Original GAUSS code by Saban Nazlioglu
- Published papers (Enders & Lee 2012; Becker et al. 2006)
- Multiple test scenarios and sample sizes

## Code Cross-Reference

### GAUSS → Python Mapping

| GAUSS Function | Python Method | Purpose |
|----------------|---------------|---------|
| `Fourier_ADF` | `FourierADF.__init__` | Main ADF test |
| `Fourier_KPSS` | `FourierKPSS.__init__` | Main KPSS test |
| `_getFourierADFCrit` | `FourierADF._get_critical_values` | ADF critical values |
| `_getFourierKPSSCrit` | `FourierKPSS._get_critical_values` | KPSS critical values |
| `_getFourierTerms` | `_create_fourier_terms` | Fourier sin/cos |
| `_get_fourier` | `FourierKPSS._run_test` | Frequency via min SSR |
| `_runFourierOLS` | `_ols_regression` | OLS estimation |
| `_get_lag` | `_select_lag` | Lag selection (ADF) |

## Acknowledgments

This Python implementation is based on the original GAUSS code by:
- **Saban Nazlioglu** (Pamukkale University, Turkey)
- Email: snazlioglu@pau.edu.tr

The methods were developed by:
- **Walter Enders** (University of Alabama)
- **Junsoo Lee** (University of Alabama)
- **Ralf Becker** (Manchester University)

## Citation

When using this package, please cite both the original papers and the GAUSS implementation:

```bibtex
@article{enders2012fourier,
  title={The flexible Fourier form and Dickey-Fuller type unit root tests},
  author={Enders, Walter and Lee, Junsoo},
  journal={Economics Letters},
  volume={117},
  number={1},
  pages={196--199},
  year={2012},
  publisher={Elsevier}
}

@article{becker2006stationarity,
  title={A stationarity test in the presence of an unknown number of smooth breaks},
  author={Becker, Ralf and Enders, Walter and Lee, Junsoo},
  journal={Journal of Time Series Analysis},
  volume={27},
  number={3},
  pages={381--409},
  year={2006},
  publisher={Wiley Online Library}
}

@software{funitroot2024,
  author = {Roudane, Merwan},
  title = {funitroot: Fourier Unit Root Tests for Python},
  year = {2024},
  note = {Based on original GAUSS implementation by Saban Nazlioglu},
  url = {https://github.com/merwanroudane/funitroot}
}
```

## License

This Python implementation is released under the MIT License.

The original GAUSS code is:
- For public non-commercial use only
- Copyright by Saban Nazlioglu

---

**Last Updated:** November 10, 2024  
**Package Version:** 1.0.0
