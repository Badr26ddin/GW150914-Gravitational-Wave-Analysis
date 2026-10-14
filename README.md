# GW150914: Exploratory Gravitational-Wave Analysis

An educational Python project exploring the public strain data around **GW150914**, the gravitational-wave event associated with a binary black-hole merger. The notebook uses matched filtering, a power chi-squared consistency test, and an off-source background scan to compare the event with noise transients in the two LIGO detectors.

> **Scope:** This is an exploratory analysis, not a reproduction of the complete LIGO–Virgo–KAGRA detection pipeline. It does not estimate an official false-alarm rate or detection significance.

## Project Goals

- Retrieve and inspect public strain data for the Hanford (**H1**) and Livingston (**L1**) detectors.
- Estimate the noise power spectral density (PSD).
- Apply a matched filter using an exploratory `IMRPhenomD` waveform template.
- Check candidate consistency with a power chi-squared statistic and calculate a reweighted SNR.
- Compare the event with a finite set of off-source background windows.
- Inspect a loud background transient using a Q-transform.

## Methods at a Glance

1. **Data:** Download 4,096 seconds of H1 and L1 strain data at 4,096 Hz using GWpy's access to public GWOSC data.
2. **Noise Estimate:** Estimate the PSD with Welch's method using 4-second segments, a 2-second stride, and median averaging.
3. **Conditioning:** High-pass filter the analysis segments at 15 Hz and use a 20 Hz low-frequency cutoff in matched filtering.
4. **Waveform Template:** Generate one `IMRPhenomD` template with `mass1 = 40` and `mass2 = 32` (in solar-mass units as used by PyCBC), with a 20 Hz lower frequency cutoff. These are exploratory choices, not fitted source parameters.
5. **Event Analysis:** Analyze a 32-second segment around the reference GPS time `1126259462.43`.
6. **Background Scan:** Analyze 251 windows per detector, each 32 seconds long with a 16-second stride, excluding windows overlapping the event's 32-second interval.
7. **Consistency Test:** Evaluate the power chi-squared statistic with 16 bins (30 degrees of freedom) and calculate reweighted SNR.
8. **Transient Inspection:** Inspect the loudest candidate background transient with a Q-transform and supporting data-quality checks.

## Main Results

The values below are from this notebook's exploratory run and depend on its template, PSD estimate, filtering, and window choices.

### GW150914 Candidate

| Detector | Matched-filter Peak SNR | Reduced Chi-squared | Reweighted SNR |
|---|---:|---:|---:|
| H1 | 19.569 | 1.166 | 18.750 |
| L1 | 13.532 | 0.756 | 13.532 |

### Background Comparison

| Detector | Background Windows | Maximum Background Reweighted SNR | Event Reweighted SNR | Windows at or Above Event Value |
|---|---:|---:|---:|---:|
| H1 | 251 | 5.493 | 18.750 | 0 / 251 |
| L1 | 251 | 5.966 | 13.532 | 0 / 251 |

No window in this finite scan reached the event's reweighted SNR in either detector. This indicates that the event stands out relative to the specific off-source windows examined; it is **not** a formal false-alarm probability.

### Loud L1 Background Transient

A background transient in L1 had a raw peak SNR of approximately `15.318`, but a reduced chi-squared of approximately `16.928`; its reweighted SNR decreased to approximately `4.179`. At the same GPS time, the H1 peak SNR was approximately `1.465` and its reweighted SNR was approximately `1.334`.

This is consistent with the L1 candidate being poorly matched to the selected waveform template and lacking a comparably strong H1 peak at the same time. These diagnostics alone do not establish the transient's physical cause.

## Visualizations

The notebook includes plots and tables for:

- H1/L1 noise amplitude spectral density (ASD)
- Matched-filter SNR around GW150914
- Background SNR distributions
- Q-transform inspection of the loud L1 transient
- Peak SNR versus reweighted SNR
- GW150914 versus the background reweighted-SNR distributions

## Requirements

The notebook installs its main dependencies in the first setup cell:

- Python, running in Google Colab or a compatible Jupyter environment
- NumPy
- pandas
- Matplotlib
- [GWpy](https://gwpy.github.io/)
- [PyCBC](https://pycbc.org/)
- [GWOSC](https://gwosc.org/) public data access through GWpy/GWOSC tools

## How to Run

### Google Colab

1. Upload `gw150914_detection_validation_reviewed.ipynb` to [Google Colab](https://colab.research.google.com/).
2. Select **Runtime → Run all** in a fresh runtime.
3. Allow the dependency-installation and data-download cells to finish. Internet access is required to retrieve public data.
4. The background reweighting scan is more computationally intensive than the plotting cells and may take longer.

Run the notebook from top to bottom. Later cells depend on objects created by earlier cells.

## Limitations and Interpretation

- Only one exploratory waveform template is used; this is not a template-bank search or a parameter-estimation analysis.
- The numerical SNR values depend on the selected waveform parameters, data conditioning, PSD estimate, and chi-squared configuration.
- The PSD is interpolated to the 32-second analysis grid for compatibility. Interpolation changes the frequency grid but does not improve the original PSD's true spectral resolution.
- Background windows overlap in their raw data, so the 251 windows per detector should not be treated as 251 independent statistical samples.
- A finite background scan with zero windows above the event threshold does not mean the false-alarm probability is zero and does not establish official detection significance.
- A large chi-squared value can indicate poor consistency with the selected template, but does not by itself identify the cause of a transient.
- This project is intended for learning and reproducible exploration, not for making an independent discovery claim.

## Repository Layout

```text
GW150914-Gravitational-Wave-Analysis/
├── README.md
└── gw150914_detection_validation_reviewed.ipynb
```

## Data and References

- **Gravitational-wave open data:** [Gravitational Wave Open Science Center (GWOSC)](https://gwosc.org/)
- **GWpy documentation:** [gwpy.github.io](https://gwpy.github.io/)
- **PyCBC documentation:** [pycbc.org](https://pycbc.org/)

## License

No explicit license is included in this repository by default. Add a `LICENSE` file if you want to specify how others may reuse your original code and documentation. Review the relevant upstream terms for external libraries and data resources.
