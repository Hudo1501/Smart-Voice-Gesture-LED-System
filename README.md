# Voice & Gesture Controlled LED System

## Overview

This project focuses on the design, implementation, and evaluation of a smart contactless lighting control system integrating Voice Recognition, Gesture Recognition, and IoT-based remote monitoring. The system is built around the ESP32 NodeMCU platform and combines Edge AI processing with embedded hardware control to deliver a responsive Human-Machine Interface (HMI) for modern smart environments.

The system supports:
- Voice-controlled lighting using an embedded AI model.
- Gesture-based interaction using optical motion sensing.
- Real-time Web App monitoring and control via Socket.IO.
- High-power LED actuation through isolated relay switching.
- RGB LED visual feedback for system status indication.

---

## System Demonstration

![image](https://github.com/Hudo1501/Smart-Voice-Gesture-LED-System/blob/main/phy_system-1.png)
---

## Tools & Components

* **Microcontroller:** ESP32 NodeMCU LuaNode32
* **Audio Sensor:** INMP441 I2S MEMS Microphone
* **Gesture Sensor:** PAJ7620U2 Optical Gesture Sensor
* **Power Module:** LM2596 DC-DC Buck Converter
* **Lighting Devices:** WS2812B RGB LED Strip, 12V 10W High-Power LED
* **Switching Device:** 1-Channel Relay Module
* **PCB Design:** Proteus Design Suite
* **AI Platform:** Edge Impulse
* **Communication Protocols:** I2S, I2C, WiFi, WebSockets

---

## System Architecture

The system follows a modular architecture consisting of four major subsystems:

1. **Power Management Block**
   - LM2596 DC-DC Buck Converter
   - 12V to 5V power regulation

2. **Data Acquisition Block**
   - INMP441 MEMS microphone
   - PAJ7620U2 gesture sensor

3. **Central Processing Block**
   - ESP32 NodeMCU
   - Edge AI inference engine
   - WiFi & WebSocket communication

4. **Execution Block**
   - WS2812B RGB LEDs
   - Relay-controlled 12V high-power LED

---

## System Block Diagram

![image](https://github.com/Hudo1501/Smart-Voice-Gesture-LED-System/blob/main/system_block_diagram-1.png)

---

## Artificial Intelligence Pipeline

### Dataset Construction

The audio dataset was collected directly using the INMP441 MEMS microphone to ensure consistency between training and deployment environments.

Supported voice commands:

* **bat_led** → Turn ON LED
* **tat_led** → Turn OFF LED
* **den_do** → Red Light
* **den_xanh** → Green Light
* **den_vang** → Yellow Light
* **noise** → Background Noise

Dataset split ratio:
- **Training Set:** 80%
- **Testing Set:** 20%

---

## Feature Extraction

The system applies MFCC (Mel Frequency Cepstral Coefficients) extraction for audio preprocessing.

### DSP Configuration

* **Sampling Rate:** 16 kHz
* **Window Size:** 1000 ms
* **Window Stride:** 200 ms
* **FFT Length:** 512
* **MFCC Coefficients:** 13
* **Filter Banks:** 32
* **Frequency Range:** 80 Hz – 4000 Hz

---

## Neural Network Architecture

The AI model is based on a lightweight 1D Convolutional Neural Network (1D-CNN) optimized for embedded deployment.

### Model Structure

* 2 Convolution Layers
  * 8 Filters
  * 16 Filters
* Dropout Layer: 0.25
* Training Epochs: 100
* Quantization: int8

The trained model is exported as an ultra-lightweight C++ library and embedded directly into ESP32 Flash memory.

---

## AI Model Performance

<p align="center">
  <img src="images/model_performance.png" width="700"/>
</p>

<p align="center">
  <em>Figure 5. Accuracy, Loss, and On-device Inference Performance</em>
</p>

---

## Communication Protocols

### I2S Protocol

The I2S protocol is used for high-fidelity digital audio transmission between:
- INMP441 Microphone
- ESP32 Microcontroller

### Signal Lines

| Signal | Function |
|---|---|
| SCK | Serial Clock |
| WS | Word Select |
| SD | Serial Data |

---

### I2C Protocol

The I2C bus is used for communication between:
- ESP32
- PAJ7620U2 Gesture Sensor

### Signal Lines

| Signal | Function |
|---|---|
| SDA | Serial Data |
| SCL | Serial Clock |

The sensor additionally provides an INT interrupt pin for event-driven gesture detection.

---

## Schematic Design

The circuit was designed following mixed-signal design principles to ensure safe isolation between:
- Low-voltage logic circuitry (3.3V / 5V)
- High-power execution circuitry (12V)

---

## Overall Schematic Diagram

<p align="center">
  <img src="images/schematic.png" width="900"/>
</p>

<p align="center">
  <em>Figure 6. Overall Circuit Schematic</em>
</p>

---

## PCB Layout

The PCB layout separates:
- High-current power traces
- Sensitive signal-processing regions

### PCB Optimization Techniques

* Wide power traces (1.5mm – 2.5mm)
* Ground Polygon Pour
* EMI shielding techniques
* Heat isolation for power modules
* Optimized routing for signal integrity

---

## PCB 2D Layout

<p align="center">
  <img src="images/pcb_2d.png" width="800"/>
</p>

<p align="center">
  <em>Figure 7. PCB 2D Routing Layout</em>
</p>

---

## PCB 3D Visualization

<p align="center">
  <img src="images/pcb_3d.png" width="800"/>
</p>

<p align="center">
  <em>Figure 8. PCB 3D Perspective View</em>
</p>

---

## System Algorithms

### Main System Flow

The software architecture operates using an event-driven mechanism.

Main tasks:
- WiFi & WebSocket handling
- Gesture monitoring
- Voice inference execution
- Relay control
- RGB LED state updates

---

## Web Application Interface

The system includes a responsive Web Application for remote monitoring and control.

### Features

* Real-time LED control
* Color Picker interface
* Brightness adjustment slider
* Voice command support using Web Speech API
* Live synchronization using Socket.IO

The Frontend source code is embedded directly into ESP32 Flash memory using:

```cpp
PROGMEM
```

---

## Web Dashboard

<p align="center">
  <img src="images/web_interface.png" width="850"/>
</p>

<p align="center">
  <em>Figure 10. Central Control Web Interface</em>
</p>

---

## Experimental Results

| Parameter | Result |
|---|---|
| Gesture Response Latency | < 200 ms |
| AI Model Accuracy | 88.2% |
| Inference Time | 10 ms |
| Peak RAM Usage | 3.8 KB |
| Flash Usage | 31.2 KB |

---

## Memory Evaluation

### Post-Boot Memory Status

* Free Heap: 208.65 KB
* Minimum Free Heap: 207.17 KB

### Full-Load Execution

* Free Heap: 177.95 KB
* Minimum Free Heap: 155.07 KB

The system demonstrated:
- Stable memory allocation
- No memory fragmentation
- No buffer overflow
- Reliable continuous operation

---

## ESP32 Memory Monitoring

<p align="center">
  <img src="images/memory_report.png" width="800"/>
</p>

<p align="center">
  <em>Figure 11. ESP32 Dynamic Memory Evaluation</em>
</p>

---

## Demo Video

[Watch System Demonstration](https://youtube.com/your-demo-link)

---

## Future Development

Several future improvements are proposed:

* Larger and more diverse voice datasets
* Active Noise Cancellation (ANC)
* Computer Vision-based gesture recognition
* MQTT cloud integration
* Dedicated mobile application
* Advanced Edge AI optimization
* Smart Home ecosystem integration


---

## References

1. ESP32 Series Datasheet – Espressif Systems
2. WS2812B Intelligent Control LED Datasheet
3. APDS-9960 Gesture Sensor Datasheet
4. Edge Impulse Documentation
