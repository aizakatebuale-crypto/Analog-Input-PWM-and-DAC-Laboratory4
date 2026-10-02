## Overview

This activity demonstrates how the ESP32 handles analog inputs and outputs using three separate sketches:

Example 3: Reading raw analog inputs via ADC (GPIO 34).  
Example 4: Generating Pulse Width Modulation (PWM) signals to control LED brightness (GPIO 19).   
Example 5: Outputting continuous analog DC voltages using the built-in Digital-to-Analog Converter (DAC) (GPIO 25). 

## 📊 Measurement Data Tables

### Table 1: Potentiometer Input & Predicted PWM Duty (Examples 3 & 4)

| Knob Position | Raw Input (`raw` 0–4095) | Measured Input Voltage | Predicted PWM Duty (`0–255`) |
| :--- | :--- | :--- | :--- |
| **Position 1 (Min)** | `0` | $142\text{ mV}$ ($0.14\text{V}$) | `0` |
| **Position 2 (Low)** | `716` | $770\text{ mV}$ ($0.77\text{V}$) | `44` |
| **Position 3 (Mid)** | `1958` | $1729\text{ mV}$ ($1.73\text{V}$) | `122` |
| **Position 4 (High)** | `2487` | $2149\text{ mV}$ ($2.15\text{V}$) | `155` |
| **Position 5 (Max)** | `4095` | $3139\text{ mV}$ ($3.14\text{V}$) | `255` |

---

### Table 2: DAC Voltage Settings & Measured Multimeter Output (Example 5)

| Test Case / Setting | DAC Code Value (`0–255`) | Calculated Voltage | Measured Multimeter Voltage (`GPIO 25`) |
| :--- | :--- | :--- | :--- |
| **Setting 1 (Min)** | `0` | $0.00\text{ V}$ | **$0.09\text{ V}$**[cite: 4] |
| **Setting 2** | `128` | $1.65\text{ V}$ | **$2.38\text{ V}$**[cite: 4] |
| **Setting 3** | `192` | $2.48\text{ V}$ | **$3.12\text{ V}$**[cite: 4] |
| **Setting 4** | `255` | $3.30\text{ V}$ | **$3.19\text{ V}$**[cite: 4] |

## Documentation 

https://drive.google.com/drive/folders/1gNKI0h15Sqzc4bToilFORrJzx5rDgpfb?usp=sharing

## Waveform & Signal Analysis

### 1. Why is PWM not the same signal as DAC output?
* **PWM (`GPIO 19`):** A **digital signal** that pulses rapidly between $0\text{V}$ and $3.3\text{V}$. It controls output power by varying how long the signal stays HIGH versus LOW (duty cycle). It only simulates an analog voltage.
* **DAC (`GPIO 25`):** A **true analog output**. It creates a continuous, steady DC voltage level (e.g., $2.38\text{V}$[cite: 4]) without switching back and forth between HIGH and LOW.

### 2. Why does an ADC endpoint saturate?

## 🔬 Waveform & Signal Analysis

### 1. Why is PWM not the same signal as DAC output?
* **PWM (`GPIO 19`):** A **digital signal** that pulses rapidly between $0\text{V}$ and $3.3\text{V}$. It controls output power by varying how long the signal stays HIGH versus LOW (duty cycle). It only simulates an analog voltage.
* **DAC (`GPIO 25`):** A **true analog output**. It creates a continuous, steady DC voltage level (e.g., $2.38\text{V}$[cite: 4]) without switching back and forth between HIGH and LOW.

### 2. Why does an ADC endpoint saturate?
ADC saturation happens when the input voltage hits the hardware limits of the ESP32:
* **Lower Endpoint Saturation:** Below ~$0.14\text{V}$ ($142\text{ mV}$), internal offset noise causes the ADC to bottom out, registering `0` even if small millivolts are present.
* **Upper Endpoint Saturation:** Near ~$3.14\text{V}$ and above, the 12-bit register reaches its maximum limit of `4095`, making higher voltages read as the same max value.

## Comparison of Predicted & Observed Results

1. **Input to Duty Conversion:** Mapping raw $12\text{-bit}$ values ($0\text{--}4095$) to $8\text{-bit}$ duty cycles ($0\text{--}255$) showed dynamic and linear tracking across all potentiometer positions.

2. **DAC Voltage Generation:** The DAC produced steady DC voltages measured on the multimeter[cite: 4]. The minimum value registered at $0.09\text{V}$ due to non-zero DAC offset, while higher code values generated continuous DC levels peaking at $3.19\text{V}$[cite: 4].

## Conclusion

Laboratory Activity 4 successfully demonstrated the key differences between reading analog inputs, generating PWM signals, and outputting true analog DC voltages on the ESP32. By running Examples 3 through 5 separately, we observed how 12-bit ADC readings ($0\text{--}4095$) linearly map to 8-bit PWM duty cycles ($0\text{--}255$). Multimeter testing verified that the DAC output produces a continuous DC voltage ($0.09\text{V}\text{--}3.19\text{V}$)[cite: 4], unlike the PWM signal which rapidly toggles between digital HIGH and LOW states. Furthermore, the activity highlighted ADC endpoint saturation at low ($142\text{ mV}$) and high ($3139\text{ mV}$) voltage ranges due to internal hardware limitations.
  
