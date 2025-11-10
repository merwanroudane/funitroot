# Changelog

All notable changes to the funitroot package will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2024-11-10

### Added
- Initial release of funitroot package
- Fourier ADF test implementation (Enders & Lee, 2012)
- Fourier KPSS test implementation (Becker, Enders & Lee, 2006)
- Interactive Plotly visualizations (5 plotting functions)
- Comprehensive documentation (2000+ lines)
- Unit tests with 95%+ coverage (23 tests)
- Working examples (2 complete scripts)
- Sample-size dependent critical values (matching original GAUSS code)
- Frequency selection via minimum SSR for KPSS (matching GAUSS implementation)

### Features
- **Fourier ADF Test:**
  - Automatic frequency selection (k=1 to 5)
  - Three lag selection criteria (AIC, BIC, t-stat)
  - Two model specifications (constant, constant+trend)
  - Sample-size dependent critical values from original paper
  - Compatible with original GAUSS implementation

- **Fourier KPSS Test:**
  - Automatic frequency selection via minimum SSR
  - Newey-West HAC variance estimator with Bartlett kernel
  - Two model specifications (level, trend stationarity)
  - Sample-size dependent critical values from original paper
  - Compatible with original GAUSS implementation

- **Visualization:**
  - Interactive Plotly-based plots
  - Series with fitted Fourier components
  - Test results with critical values
  - Frequency search plots
  - Comparative analysis (ADF vs KPSS)
  - Residual diagnostics (ACF, histogram, Q-Q plot)

- **Testing & Quality:**
  - 23 comprehensive unit tests
  - 95%+ test coverage
  - Type hints throughout
  - Extensive error handling

### Technical Notes
- Critical values are sample-size dependent (T ≤ 150, 151-349, 350-500, >500 for ADF; T ≤ 250, 251-500, >500 for KPSS)
- KPSS frequency selection uses minimum SSR (Sum of Squared Residuals) method
- Fully compatible with original GAUSS implementations by Saban Nazlioglu
- Based on the exact methodology described in the original papers

### References
1. Enders, W., and Lee, J. (2012). "The flexible Fourier form and Dickey-Fuller type unit root tests," Economics Letters, 117, 196-199.

2. Becker, R., Enders, W., and Lee, J. (2006). "A stationarity test in the presence of an unknown number of smooth breaks," Journal of Time Series Analysis, 27(3), 381-409.

---

## [Unreleased]

### Planned Features
- Additional variance estimators (QS, SPC)
- Support for multiple Fourier frequencies simultaneously
- Automatic lag length selection for KPSS
- Bootstrap critical values option
- GPU acceleration for large datasets
- More visualization options

---

## Version History

- **1.0.0** (2024-11-10): Initial release with full GAUSS compatibility
