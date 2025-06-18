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

🧪 How It Works
1. Load and Preprocess Data
data = pd.read_csv('All_data.csv')
fsr_data = data['F-volt'].values[1601:1801]

2. Bandpass Filter (5–200 Hz)
from scipy.signal import butter, filtfilt

def butter_bandpass_filter(data, lowcut, highcut, fs, order):
    nyq = 0.5 * fs
    low = lowcut / nyq
    high = highcut / nyq
    b, a = butter(order, [low, high], btype='band')
    return filtfilt(b, a, data)

3. Spike Encoding
FSR (FA1 and SA1)
fa1_spikes = np.where(filtered_signal > threshold)[0] / fs
sa1_spikes = np.where(filtered_signal < -threshold)[0] / fs

Vibration (FA2)
vib_fm = np.sin(2*np.pi*(carrier_freq + modulation_index * filtered_signal)*t)
vib_spikes = np.where(vib_fm > threshold)[0] / fs
Softpot (SA2)
sa2_spikes = np.where(filtered_softpot < -threshold)[0] / fs

📊 Visualization
Raw time-series data
Raster plot of spike trains (color-coded by receptor type)
fig, ax = plt.subplots(figsize=(15, 5))
ax.eventplot([fa1, sa1, vib, sa2], colors=['r', 'g', 'b', 'm'])
