How It Works
Data Collection & Labeling (labeldata.py)
Raw sensor readings are labeled using rule-based thresholds:
Gas > 400 and water detected → danger
Gas > 400 only → gas_leak
Water detected only → water_high
Otherwise → normal
Model Training
The labeled dataset is used to train a classification model (saved as model/gas_water_model.pkl), which predicts the safety status from gas and water sensor values.
Real-Time Monitoring (server.py / run_system.py)
Sensor data streams in from the ESP8266 over a serial connection
The trained model predicts the current status along with a confidence score
server.py runs this in a background thread and serves live results at /predict as prediction,confidence

Technologies Used
Hardware: ESP8266, MQ-2 Gas Sensor, Water Level Sensor
Backend: Python, Flask
Machine Learning: scikit-learn (via joblib model loading), pandas, NumPy
Communication: Serial (pySerial)

Setup & Usage
Install dependencies:
Code
pip install flask pyserial joblib pandas numpy scikit-learn
Connect the ESP8266 to your machine and note the COM port (update COM3 in server.py / run_system.py if different)
Run the Flask server:
Code 
python server.py
Access live predictions at:
Code
http://localhost:5000/predict

Project Background
Developed as an academic group project. Contributed to system design, implementation, testing, documentation, and presentation. Presented at a college innovation exhibition — 2nd Place, Launch Pad Inventors Expo (2024–2025).

