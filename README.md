# 🐝 Honey_Pot — ML-Based Detection Framework for IoT DDoS Attacks

A honeypot-driven, machine learning powered system that simulates vulnerable IoT/enterprise devices, captures live attacker activity, and classifies traffic as **normal** or **DDoS/malicious** in real time. The project pairs a [Cowrie](https://github.com/cowrie/cowrie)-based honeypot (deployed in an isolated Docker environment) with a supervised ML pipeline built on Scikit-learn to automatically fingerprint botnet-driven DDoS behavior from captured logs.

---

## 📌 Overview

IoT devices are a common entry point for botnet-driven DDoS attacks because they are frequently deployed with weak credentials and minimal monitoring. This project addresses that gap by:

1. **Deploying a honeypot** that mimics vulnerable IoT/enterprise devices inside an isolated Docker container, safely luring and logging real attacker interactions (login attempts, command input, session activity).
2. **Capturing and preprocessing** the resulting traffic/log data into structured features.
3. **Training and evaluating multiple ML classifiers** to distinguish normal traffic from malicious/DDoS activity.
4. **Classifying new, unseen honeypot logs** in real time using the trained model, reducing false positives and enabling automated threat fingerprinting.

---

## ✨ Key Features

- 🐳 **Containerized Honeypot Deployment** — Docker-based isolation creates a controlled, reproducible threat environment for safely capturing live attack traffic.
- 📊 **Automated Log Preprocessing** — Parses raw honeypot session logs (Cowrie JSON event logs) into clean, labeled datasets (`dataset.txt`, `process.csv`).
- 🤖 **Multi-Model ML Pipeline** — Trains and benchmarks several supervised classifiers on the same dataset:
  - Random Forest
  - Support Vector Machine (SVM)
  - K-Nearest Neighbors (KNN)
  - Decision Tree
  - Neural Network (Keras/TensorFlow)
- 📈 **Performance Evaluation** — Computes Accuracy, F1-score, and ROC-AUC for every model, with comparison graphs to identify the best-performing classifier.
- 🔍 **Real-Time Attack Prediction** — Loads a new honeypot log file and classifies each session as a *normal request* or *DDoS attack* using the trained model.
- 🖥️ **Interactive Desktop GUI** — A Tkinter-based interface ties the entire workflow together: upload logs → preprocess → train → evaluate → visualize → predict.

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Honeypot | Cowrie (simulated IoT/enterprise device), Docker |
| Language | Python |
| ML / Data | Scikit-learn, Keras/TensorFlow, Pandas, NumPy |
| Visualization | Matplotlib |
| GUI | Tkinter |
| Network Capture | Wireshark |

---

## 📂 Project Structure

```
Honey_Pot/
├── Honeypot_log_dataset/   # Raw honeypot session logs used for training/testing
├── HoneypotML.py           # Main application: GUI, preprocessing, model training & prediction
├── dataset.txt             # Preprocessed dataset generated from honeypot logs
├── process.csv             # Encoded dataset ready for ML training
├── newdata.txt             # Preprocessed data for new/unseen logs
├── newprocess.csv          # Encoded dataset for prediction on new logs
├── run.bat                 # Windows script to launch the application
└── README.md
```

---

## ⚙️ How It Works

1. **Capture** — The Cowrie honeypot, deployed in an isolated Docker container, simulates a vulnerable device and logs every attacker interaction (login attempts, command execution, session activity) as JSON events.
2. **Preprocess** — `HoneypotML.py` parses these logs, extracts relevant features (event type, source IP, command/login outcome), encodes categorical fields, and labels each record as *clean*, *malicious*, *spying*, or *DDoS attack*.
3. **Train** — The labeled dataset is split into training/testing sets and used to train five classifiers in parallel: Random Forest, SVM, KNN, Decision Tree, and a Keras-based Neural Network.
4. **Evaluate** — Each model's Accuracy, F1-score, and ROC-AUC are computed and plotted for direct comparison, helping identify the most reliable classifier (Random Forest was the primary focus for reducing false positives).
5. **Predict** — New, unseen honeypot logs can be uploaded and classified on the fly, flagging sessions that contain DDoS attack patterns.

---

## 🚀 Getting Started

### Prerequisites
- Python 3.7+
- Docker (for deploying the honeypot environment)
- pip packages: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `keras`, `tensorflow`

### Installation

```bash
# Clone the repository
git clone https://github.com/KoushikVatam/Honey_Pot.git
cd Honey_Pot

# Install dependencies
pip install pandas numpy scikit-learn matplotlib keras tensorflow
```

### Running the Application

```bash
python HoneypotML.py
```

Or on Windows, simply run:

```bash
run.bat
```

### Usage
1. Click **Upload Honeypot Logs & Preprocess** and select a log file from `Honeypot_log_dataset/`.
2. Run any of the classifier buttons (SVM, KNN, Decision Tree, Random Forest, Neural Network) to train and view performance metrics.
3. Click **Accuracy Graph** to compare all models, or **Attack Graph** to visualize the distribution of attack types in the dataset.
4. Click **Classify/Predict Attack from New Log** to evaluate a new, unseen log file using the trained model.

---

## 📈 Results

The pipeline benchmarks Accuracy, F1-score, and ROC-AUC across all five classifiers on captured honeypot traffic, with Random Forest used as the primary detection model to minimize false positives and reliably fingerprint DDoS-related sessions.

---

## 🔮 Future Improvements

- Automate the Docker honeypot deployment with a `docker-compose.yml` included in this repo.
- Add real-time traffic ingestion (rather than batch log upload) for live monitoring.
- Package the trained Random Forest model for deployment as a lightweight detection service/API.
- Expand feature engineering using packet-level features captured via Wireshark.

---

## 📄 License

This project is open source and available for educational and research purposes. Add a `LICENSE` file to formally specify usage terms.

---

## 👤 Author

**Koushik Vatam**
GitHub: [@KoushikVatam](https://github.com/KoushikVatam)
