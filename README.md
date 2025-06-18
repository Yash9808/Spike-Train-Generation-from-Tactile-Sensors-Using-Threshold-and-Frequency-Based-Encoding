# Spike-Train-Generation-from-Tactile-Sensors-Using-Threshold-and-Frequency-Based-Encoding

This repository implements a biologically inspired system that converts analog tactile sensor data into spike trains. The goal is to simulate skin mechanoreceptors and explore neuromorphic encoding methods using Python.

## 🔍 Overview

This project demonstrates how to:

- Read analog data from FSR, vibration, and softpot sensors
- Apply biologically plausible bandpass filtering
- Encode signals into spike trains using:
  - **Threshold-based encoding** (FA1, SA1, SA2)
  - **Frequency Modulation (FM)** encoding (FA2)
- Visualize spike output via raster plots

---

## 🧠 Biological Inspiration

Four types of mechanoreceptors are mimicked:
- **FA1**: Fast-adapting, detects dynamic pressure (positive threshold)
- **SA1**: Slow-adapting, detects static pressure (negative threshold)
- **FA2**: Vibration-sensitive, encoded using frequency modulation
- **SA2**: Position/pressure tracking using slow-adapting threshold encoding

---

## 📁 Files

- `All_data.csv`: Sensor dataset (FSR, Vibration, Softpot)
- `main.py`: Main script containing all processing steps
- `README.md`: You’re reading it :)

---

## 📦 Dependencies

Make sure to install the required Python packages:

```bash
pip install numpy matplotlib pandas scipy
