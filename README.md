# AI Safety Beacon — IoT-Based Safety Monitoring System

An IoT-based safety monitoring system that reads live sensor data (gas and water level) from an ESP8266 microcontroller, applies a trained machine learning model to detect hazardous conditions in real time, and exposes the results through a Flask web API.

## Overview

The system continuously monitors two safety-critical parameters:
- **Gas levels** — using an MQ-2 gas sensor
- **Water levels** — using a water level sensor

Sensor readings are sent from the ESP8266 over serial connection to a Python backend, where a trained ML model classifies the current state as normal, gas_leak, water_high, or danger.

## Project Structure

**labeldata.py** — Labels raw sensor data (sensor_data.csv) into categories (normal, gas_leak, water_high, danger) based on threshold rules, producing labeled_data.csv for model training

**run_system.py** — Standalone script that reads live serial data from the ESP8266 and prints real-time predictions to the console

**server.py** — Flask web server that runs the same prediction pipeline in a background thread and exposes live predictions via a /predict API endpoint

## How It Works

1. Data Collection & Labeling (labeldata.py)
Raw sensor readings are labeled using rule-based thresholds:
- Gas > 400 and water detected leads to danger
- Gas > 400 only leads to gas_leak
- Water detected only leads to water_high
- Otherwise leads to normal

2. Model Training
The labeled dataset is used to train a classification model (saved as model/gas_water_model.pkl), which predicts the safety status from gas and water sensor values.

3. Real-Time Monitoring (server.py / run_system.py)
- Sensor data streams in from the ESP8266 over a serial connection
- The trained model predicts the current status along with a confidence score
- server.py runs this in a background thread and serves live results at /predict as prediction,confidence

## Technologies Used

- Hardware: ESP8266, MQ-2 Gas Sensor, Water Level Sensor
- Backend: Python, Flask
- Machine Learning: scikit-learn (via joblib model loading), pandas, NumPy
- Communication: Serial (pySerial)

## Setup & Usage

1. Install dependencies: pip install flask pyserial joblib pandas numpy scikit-learn
2. Connect the ESP8266 to your machine and note the COM port (update COM3 in server.py / run_system.py if different)
3. Run the Flask server: python server.py
4. Access live predictions at: http://localhost:5000/predict

## Project Background

Developed as an academic group project. Contributed to system design, implementation, testing, documentation, and presentation. Presented at a college innovation exhibition — 2nd Place, Launch Pad Inventors Expo (2024–2025).