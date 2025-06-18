# Spike-Train-Generation-from-Tactile-Sensors-Using-Threshold-and-Frequency-Based-Encoding
This document explains a Python-based implementation of a spike encoding system that converts analog sensor readings into spike trains using biologically inspired mechanisms. The system is designed to simulate different types of skin mechanoreceptors and supports temporal neural encoding. 
1. Libraries and Setup

import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from scipy.signal import butter, filtfilt

numpy: For numerical operations

matplotlib.pyplot: For data visualization

pandas: For loading and manipulating sensor data

scipy.signal: For signal filtering using Butterworth filters

2. Sampling Configuration

fs = 1000  # Sampling frequency in Hz
t = np.arange(0, 0.2, 1/fs)  # 200 ms total duration

fs (sampling frequency) defines how many data points are collected per second.

t (time vector) spans from 0 to 200 ms with 1 ms resolution.

3. Load Sensor Data

data90 = pd.read_csv('All_data.csv')
fsr_data = data90['F-volt'].values[1601:1801]
vib_data = data90['V-volt'].values[1601:1801]
softpot_data = data90['P-volt'].values[1601:1801]

Loads sensor voltage data for three modalities:

FSR (Force Sensing Resistor)

Vibration Sensor

Softpot (Position/Pressure sensing)

A specific time segment (1601 to 1801 samples) is extracted.

4. Signal Filtering

lowcut = 5
highcut = 200
order = 2

Defines a bandpass filter that allows signals between 5 Hz and 200 Hz (common neural signal range).

def butter_bandpass_filter(data90, lowcut, highcut, fs, order):
    nyq = 0.5 * fs
    low = lowcut / nyq
    high = highcut / nyq
    b, a = butter(order, [low, high], btype='band')
    y = filtfilt(b, a, data90)
    return y

Implements a zero-phase Butterworth bandpass filter.

Ensures biological plausibility by removing slow drifts and high-frequency noise.

5. Spike Encoding for FSR (FA1 and SA1 types)

fsr_filtered = butter_bandpass_filter(fsr_data, lowcut, highcut, fs, order)
fsr_receptor_potential = fsr_filtered - np.mean(fsr_filtered)
fa1_spikes = np.where(fsr_receptor_potential > threshold)[0] / fs
sa1_spikes = np.where(fsr_receptor_potential < -threshold)[0] / fs

Converts the filtered FSR signal into spike trains:

FA1 (Fast-Adapting): Positive threshold crossings

SA1 (Slow-Adapting): Negative threshold crossings

This mimics mechanoreceptors in skin with different adaptation rates.

6. FM Encoding for Vibration Sensor (FA2)

vib_filtered = butter_bandpass_filter(vib_data, lowcut, highcut, fs, order)
vib_carrier = np.sin(2*np.pi*fm_carrier_freq*t)
vib_modulation = fm_modulation_index * vib_filtered
vib_fm = vib_carrier * np.sin(2*np.pi*(fm_carrier_freq+vib_modulation)*t)
vib_spikes = np.where(vib_fm > threshold)[0] / fs

FM (Frequency Modulation) encodes signal strength into carrier frequency.

Spikes are generated when FM signal exceeds threshold.

FA2 receptors respond to high-frequency vibration; FM encoding simulates this dynamic sensitivity.

7. Threshold-Based Encoding for Softpot Sensor (SA2)

softpot_filtered = butter_bandpass_filter(softpot_data, lowcut, highcut, fs, order)
softpot_receptor_potential = softpot_filtered - np.mean(softpot_filtered)
sa2_spikes = np.where(softpot_receptor_potential < -threshold)[0] / fs

Applies a simple threshold mechanism to simulate SA2-type slowly adapting receptors.

No modulation is used; spike events are triggered when the potential falls below a negative threshold.

8. Visualization

Time-Series Plots

fig, axs = plt.subplots(4, 1, figsize=(12, 12), sharex=True)
axs[0].plot(t, fsr_data, 'k')
axs[1].plot(t, vib_data, 'k')
axs[2].plot(t, softpot_data, 'k')

Visualizes raw data from each sensor.

Raster Plot of Spike Trains

all_spikes = [fa1_spikes, sa1_spikes, vib_spikes, sa2_spikes]
fig, ax = plt.subplots(figsize=(15, 5))
colors = ['r', 'g', 'b', 'm']
for i, spikes in enumerate(all_spikes):
    ax.eventplot(spikes, color=colors[i], lineoffsets=i+1, linelengths=0.8)
ax.set_yticks([1, 2, 3, 4])
ax.set_yticklabels(['FA1', 'SA1', 'Vibration (FA2)','Softpot (SA2)'])

Raster plots are used in neuroscience to show spike timing across channels.

Each row represents one sensor/modality.

9. References

Dayan, P., & Abbott, L. F. (2001). Theoretical Neuroscience. MIT Press.

Gerstner, W., & Kistler, W. M. (2002). Spiking Neuron Models. Cambridge University Press.

Mead, C. (1990). Neuromorphic electronic systems. Proceedings of the IEEE.

Thorpe, S., & Imbert, M. (1989). Biological constraints on connectionist modelling. In Connectionism in perspective.

Indiveri, G., et al. (2011). Neuromorphic silicon neuron circuits. Frontiers in Neuroscience.
