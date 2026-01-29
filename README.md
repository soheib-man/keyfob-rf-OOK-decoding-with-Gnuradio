# keyfob-rf-OOK-decoding-with-Gnuradio
# 📡 RF Remote Signal Analysis Decoding Challenge

**Digital Communications Systems - Advanced Telecommunication Technologies**

**Students:** Soheib Abbessamad Miloudi  & Ahmed Fekkai   
**Date:** January 29, 2026

[![View Full Report](https://img.shields.io/badge/📄-Full_PDF_Report-blue)](rf_challenge.pdf)
[![GNU Radio](https://img.shields.io/badge/GNU-Radio-3C5B72?logo=gnuradio)](https://www.gnuradio.org/)
[![Python](https://img.shields.io/badge/Python-3.13.9+-3776AB?logo=python)](https://python.org)

## 📌 Overview

This project demonstrates the complete analysis and decoding of a garage door keyfob RF signal using **Software-Defined Radio (SDR)** techniques. We captured, analyzed, and reverse-engineered a 433.92 MHz remote control signal to understand its modulation, encoding, frame structure, and security characteristics.

**⚠️ Educational Use Only:** All analysis was performed on prerecorded signals in compliance with ethical and legal guidelines.

## 🎯 Key Findings

| Aspect | Discovery |
|--------|-----------|
| **Carrier Frequency** | 433.92 MHz ±0.05 MHz |
| **Modulation** | OOK (On-Off Keying) |
| **Bit Rate** | 3,816 bps (T_bit = 262μs) |
| **Encoding** | Pulse Width Modulation (PWM) |
| **Frame Structure** | 62 bits total (9-bit preamble + 53-bit data) |
| **Security Level** | Basic fixed-code with partial variability |

## 🛠️ Equipment & Software

### Hardware
- **USRP Software Defined Radio** (for original signal capture)
- **Prerecorded IQ samples** of garage door keyfob transmission

### Software
- **GNU Radio Companion 3.10** for signal processing
- **Python 3.13.9+** with:
  - NumPy for numerical analysis
  - Matplotlib for visualization
  - Glob for file handling

## 🔬 Methodology

### 1. Signal Acquisition & Initial Inspection
The analysis began with loading prerecorded IQ files into GNU Radio for spectral and temporal analysis.

![GNU Radio Flowgraph](docs/images/flowgraph.png)

### 2. Frequency & Power Analysis
Using QT GUI Frequency Sink, we confirmed the signal operates at **433.920 MHz** with energy concentration around the carrier.

![Spectral Analysis](docs/images/spectrum.png)
*Figure 2: Signal spectrum at 433.92 MHz*

### 3. Modulation Identification
Time-domain analysis revealed clear **On-Off Keying (OOK)** modulation with distinct binary states.

### 4. Bit Rate Determination
By measuring individual bit duration:
**T_bit = 262 μs → R_b = 3,816 bps**

### 5. Frame Structure Analysis
We extracted the complete frame using OOK decoding in GNU Radio with Complex-to-Magnitude conversion.

## 🔍 Signal Decoding Process

### Threshold Detection

A threshold detector was implemented to convert the analog envelope to digital bits using hysteresis thresholds:

- **Upper threshold (V_thresh-high):** 0.02
- **Lower threshold (V_thresh-low):** 0.010
- **Hysteresis band:** 0.01



### PWM Decoding
Discovered PWM encoding where bit values are represented by different HIGH-state durations. To handle large files efficiently, we used:

1. **Moving Average Filter** (10k samples) for frame detection
2. **Burst Tagger** + **Tagged File Sink** for efficient storage
3. Captured 6 distinct transmission frames

