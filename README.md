# Edge AI-Based Real-Time Multi-Gas Anomaly Detection System for Sewer Worker Safety

## 🏭 Project Overview

This project implements an **Edge AI-based anomaly detection system** using ESP32 microcontroller with multiple gas sensors to ensure the safety of sewer workers by detecting hazardous gas concentrations in real-time.

### Key Features
- **Real-time multi-gas monitoring**: H₂S, CH₄, CO detection
- **Edge AI processing**: Random Forest model running on ESP32
- **Anomaly detection**: Identifies abnormal gas patterns instantly
- **Visual & Audio alerts**: OLED display + RGB LED + Buzzer
- **Battery-powered**: Portable and field-ready
- **Low-latency**: No cloud dependency, instant local inference

---

## 🛠️ Hardware Components

| Component | Specification | Purpose |
|-----------|---------------|---------|
| **ESP32** | 240 MHz dual-core, 4MB SPIFFS | Main microcontroller & edge AI unit |
| **MQ-136 / TGS-2600** | H₂S sensor | Hydrogen sulfide detection (0-100 ppm) |
| **MQ-4 / TGS-2611** | CH₄ sensor | Methane detection (0-20000 ppm) |
| **MQ-7 / TGS-203** | CO sensor | Carbon monoxide detection (0-1000 ppm) |
| **DHT22** | Temperature + Humidity | Environmental context (±0.5°C, ±2% RH) |
| **SSD1306** | 128x64 OLED Display | Real-time status & readings |
| **WS2812B** | RGB LED (Neopixel) | Visual alert indicator |
| **5V Buzzer** | Audio alarm | Acoustic warning signal |
| **18650 Li-ion Battery** | 3.7V 2600mAh | Power supply with charging circuit |

---

## 💻 Software Stack

### Development Tools
- **ArduinoIDE / PlatformIO**: ESP32 firmware development
- **Python 3.8+**: ML model development & dataset processing
- **Jupyter Notebook**: Model training & visualization

### Libraries & Frameworks
- **Pandas**: Data preprocessing & feature engineering
- **NumPy**: Numerical computations
- **Scikit-learn**: Random Forest model training
- **Matplotlib/Seaborn**: Data visualization
- **Adafruit Libraries**: Sensor drivers (DHT, SSD1306)
- **M5Stack Arduino Libraries**: ESP32 sensor integration
- **TinyML / TensorFlow Lite Micro**: Edge AI inference

---

## 🤖 ML Algorithm: Random Forest

### Why Random Forest?
- **Robust anomaly detection** on multivariate sensor data
- **Fast inference** (~1-5ms per prediction)
- **Low memory footprint** (fits on ESP32 SPIFFS)
- **Interpretable feature importance** for debugging
- **Handles non-linear patterns** in gas sensor drift

### Model Architecture
```
Input Features (5):
  ├── H₂S_level (ppm)
  ├── CH₄_level (ppm)
  ├── CO_level (ppm)
  ├── Temperature (°C)
  └── Humidity (%RH)
        ↓
  Random Forest Classifier (100 trees, depth=10)
        ↓
Output: [NORMAL, WARNING, DANGER, SENSOR_FAULT]
```

---

## 📁 Project Structure

```
edge-ai-multi-gas-anomaly-detection-esp32/
│
├── 📂 hardware/
│   ├── circuit_diagram.png
│   ├── pinout_esp32.txt
│   ├── bom.csv                    # Bill of materials
│   └── assembly_guide.md
│
├── 📂 firmware/
│   ├── esp32_multi_gas_sensor/
│   │   ├── esp32_multi_gas_sensor.ino
│   │   ├── config.h               # Sensor pins & calibration
│   │   ├── sensor_driver.cpp      # MQ sensor interface
│   │   ├── dht_driver.cpp         # Temperature/humidity
│   │   ├── display_manager.cpp    # OLED display logic
│   │   ├── alert_system.cpp       # LED/Buzzer control
│   │   ├── ml_inference.cpp       # Random Forest on ESP32
│   │   ├── data_logger.cpp        # SPIFFS logging
│   │   └── platformio.ini
│   └── libraries/
│       ├── RandomForestModel.h    # Model coefficients
│       └── SensorCalibration.h    # Calibration data
│
├── 📂 ml_model/
│   ├── 01_data_collection.py      # Synthetic & real sensor data
│   ├── 02_data_preprocessing.py   # Cleaning & normalization
│   ├── 03_feature_engineering.py  # Feature extraction
│   ├── 04_model_training.py       # Random Forest training
│   ├── 05_model_evaluation.py     # Performance metrics
│   ├── 06_model_export.py         # Export for ESP32
│   ├── datasets/
│   │   ├── synthetic_sensor_data.csv
│   │   ├── field_calibration_data.csv
│   │   └── anomaly_samples.csv
│   ├── models/
│   │   ├── random_forest_model.pkl
│   │   ├── model_config.json
│   │   └── model_trees.h          # C++ header for ESP32
│   └── notebooks/
│       └── anomaly_detection_analysis.ipynb
│
├── 📂 dashboard/
│   ├── web_dashboard.html         # Real-time monitoring web UI
│   ├── mobile_app/                # Optional mobile companion app
│   └── config.json                # Dashboard settings
│
├── 📂 tests/
│   ├── unit_tests.py
│   ├── sensor_calibration_test.ino
│   └── integration_tests.py
│
├── 📂 docs/
│   ├── INSTALLATION.md            # Setup guide
│   ├── API_REFERENCE.md           # Firmware API
│   ├── TROUBLESHOOTING.md         # Common issues
│   ├── CALIBRATION_GUIDE.md       # Sensor calibration
│   └── DEPLOYMENT.md              # Production deployment
│
├── requirements.txt               # Python dependencies
├── setup.sh                       # Environment setup script
├── LICENSE
└── .gitignore
```

---

## 🚀 Quick Start

### Step 1: Hardware Assembly
1. Connect sensors to ESP32 according to `hardware/pinout_esp32.txt`
2. Assemble OLED display, RGB LED, and buzzer
3. Connect battery with charging circuit

### Step 2: Firmware Setup
```bash
cd firmware/esp32_multi_gas_sensor
# Using PlatformIO
pio init -b esp32
pio run -t upload
```

### Step 3: ML Model Training
```bash
cd ml_model
pip install -r requirements.txt
python 01_data_collection.py
python 02_data_preprocessing.py
python 04_model_training.py
python 06_model_export.py  # Export model for ESP32
```

### Step 4: Deploy on ESP32
```bash
# Copy generated model header file to firmware
cp ml_model/models/model_trees.h firmware/esp32_multi_gas_sensor/libraries/
# Re-upload firmware
pio run -t upload
```

---

## 📊 Expected Performance

| Metric | Value |
|--------|-------|
| **Inference Time** | 2-5 ms per prediction |
| **Memory Usage** | ~150 KB (ESP32 has 520 KB SRAM) |
| **Flash Storage** | ~200 KB (model + firmware) |
| **Sensor Sampling Rate** | 100 Hz (configurable) |
| **Detection Latency** | <100 ms end-to-end |
| **Power Consumption** | ~80-120 mA (active), ~5 mA (sleep) |
| **Battery Runtime** | ~20-30 hours continuous |
| **Model Accuracy** | >95% on validation dataset |

---

## 🔒 Safety Thresholds (Sewer Environment)

### Gas Concentration Limits (OSHA/NIOSH standards)
```
H₂S:
  ├── NORMAL: < 10 ppm
  ├── WARNING: 10-15 ppm
  └── DANGER: > 15 ppm

CH₄:
  ├── NORMAL: < 1000 ppm
  ├── WARNING: 1000-2000 ppm
  └── DANGER: > 2000 ppm (Explosion hazard)

CO:
  ├── NORMAL: < 35 ppm
  ├── WARNING: 35-50 ppm
  └── DANGER: > 50 ppm

O₂ (via algorithm):
  ├─�� NORMAL: 19.5-23.5%
  ├── WARNING: 16-19.5% or 23.5-25%
  └── DANGER: < 16% or > 25%
```

---

## 📚 Documentation

- **[INSTALLATION.md](docs/INSTALLATION.md)** - Complete setup & deployment guide
- **[CALIBRATION_GUIDE.md](docs/CALIBRATION_GUIDE.md)** - Sensor calibration procedures
- **[API_REFERENCE.md](docs/API_REFERENCE.md)** - Firmware API documentation
- **[TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)** - Common issues & solutions
- **[DEPLOYMENT.md](docs/DEPLOYMENT.md)** - Production deployment checklist

---

## 📖 References & Standards
- OSHA PEL (Permissible Exposure Limits)
- NIOSH REL (Recommended Exposure Limits)
- IDLH (Immediately Dangerous to Life/Health)
- Confined Space Entry Standards (29 CFR 1910.146)

---

## 👥 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request with detailed description

---

## 📝 License

MIT License - See LICENSE file for details

---

## 📞 Support & Issues

For issues, questions, or suggestions:
- Open a GitHub Issue
- Check [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
- Review documentation in `docs/`

---

**Last Updated**: October 2026
**Version**: 1.0.0
