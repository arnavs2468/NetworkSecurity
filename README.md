# 🛡️ Network Security: End-to-End Phishing Detection System

[![Python](https://img.shields.io/badge/Python-3.10%20%7C%203.11-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![MLflow](https://img.shields.io/badge/MLflow-0194E2?style=for-the-badge&logo=mlflow&logoColor=white)](https://mlflow.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB_Atlas-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)

> An industry-grade Machine Learning and MLOps pipeline that detects malicious and phishing websites in real time based on 30+ domain heuristics, URL structures, and network security indicators.

---

## 📑 Table of Contents
- [📌 Overview & Problem Statement](#-overview--problem-statement)
- [✨ Key Features](#-key-features)
- [🏗️ Pipeline & System Architecture](#️-pipeline--system-architecture)
- [📊 Dataset & Feature Highlights](#-dataset--feature-highlights)
- [⚡ Quick Start & Local Setup](#-quick-start--local-setup)
- [🧪 How to Test Predictions](#-how-to-test-predictions)
- [📈 Experiment Tracking with MLflow](#-experiment-tracking-with-mlflow)
- [🚀 Cloud Deployment (Render / Docker)](#-cloud-deployment-render--docker)
- [📂 Project Directory Structure](#-project-directory-structure)

---

## 📌 Overview & Problem Statement

Over **90% of cyber attacks** begin with deceptive phishing links designed to steal credentials, credit card details, or corporate access. Conventional URL blacklists often fail against newly registered domains and polymorphic phishing attacks.

This project implements a **modular, production-ready Machine Learning system** capable of analyzing domain structures, SSL certificate properties, DNS records, and traffic metrics to instantly classify whether a given URL is **Legitimate (`0`)** or **Phishing (`1`)**.

---

## ✨ Key Features

- **Modular MLOps Pipeline:** Fully decoupled stages for Data Ingestion, Data Validation (Schema validation & Drift detection), Data Transformation (KNN Imputation), and Model Training.
- **Multi-Model Benchmarking:** Automated training and hyperparameter tuning across **Random Forest, Gradient Boosting, AdaBoost, Decision Trees, and Logistic Regression**.
- **Remote Experiment Tracking:** Full integration with **MLflow & DagsHub** for logging metrics (F1-score, Precision, Recall) and model registry artifacts.
- **High-Speed FastAPI Microservice:** RESTful API with automated Swagger UI (`/docs`) for easy interactive testing and integration.
- **Batch CSV Inference & HTML Views:** Upload any batch `.csv` dataset and get immediate predictions formatted into styled interactive tables and downloadable CSVs.
- **Docker & Cloud Ready:** Pre-configured Dockerfile and Procfile for instant zero-hassle deployment on Render, Railway, Hugging Face, or AWS.

---

## 🏗️ Pipeline & System Architecture

```
                                  ┌───────────────────────────────┐
                                  │   MongoDB Atlas / Raw Data    │
                                  └───────────────┬───────────────┘
                                                  │
                                                  ▼
                                      ┌──────────────────────┐
                                      │    Data Ingestion    │
                                      └───────────┬──────────┘
                                                  │
                                                  ▼
                                      ┌──────────────────────┐
                                      │   Data Validation    │ ──► [ Schema & Drift Checks ]
                                      └───────────┬──────────┘
                                                  │
                                                  ▼
                                      ┌──────────────────────┐
                                      │ Data Transformation  │ ──► [ KNN Imputer & Scaler ]
                                      └───────────┬──────────┘
                                                  │
                                                  ▼
                                      ┌──────────────────────┐
                                      │    Model Trainer     │ ──► [ Model Selection & Hyperopt ]
                                      └───────────┬──────────┘
                                                  │
                         ┌────────────────────────┴────────────────────────┐
                         │                                                 │
                         ▼                                                 ▼
             ┌────────────────────────┐                        ┌───────────────────────┐
             │   MLflow on DagsHub    │                        │  FastAPI Web Service  │
             │ (Experiment Tracking)  │                        │ (Interactive /docs)   │
             └────────────────────────┘                        └───────────────────────┘
```

---

## 📊 Dataset & Feature Highlights

The model extracts and analyzes **30 distinct domain and structural attributes**:

| Category | Key Indicators Analyzed |
| :--- | :--- |
| **Address Bar Heuristics** | IP Address presence, URL Length, URL Shortening services, `@` symbol, Double-slash redirects, Hyphen (`-`) prefix/suffix, Subdomain depth |
| **Security & Certificate** | SSL Final State, HTTPS in domain token, Domain Registration Length, DNS Records |
| **Abnormal Patterns** | Non-standard ports, Anchor URL ratios, Request URLs, Links in `<tags>`, Server Form Handler (`SFH`), Email submission forms, Iframe redirects |
| **Traffic & Reputation** | Web traffic rankings, Google Index status, PageRank, Domain Age, Statistical reports |

**Target Labels:**
- `0` $\rightarrow$ **Legitimate / Safe Website**
- `1` $\rightarrow$ **Phishing / Malicious Website**

---

## ⚡ Quick Start & Local Setup

### 1. Clone the Repository
```bash
git clone https://github.com/arnavs2468/NetworkSecurity.git
cd NetworkSecurity
```

### 2. Create and Activate a Virtual Environment
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux / MacOS
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. (Optional) Set Environment Variables in `.env`
Create a `.env` file in the root directory if you want to connect to MongoDB and MLflow:
```env
MONGODB_URL_KEY="your_mongodb_connection_uri"
```

### 5. Launch the Application
```bash
python app.py
```
Open your browser and navigate to **[http://localhost:8000/docs](http://localhost:8000/docs)** to view the interactive API documentation!

---

## 🧪 How to Test Predictions

1. Start the server (`python app.py`) and visit **`http://localhost:8000/docs`**.
2. Click on the **`POST /predict`** endpoint and click **"Try it out"**.
3. Upload the included sample file: **`sample_test_data.csv`**.
4. Click **Execute**.
5. The API will return an HTML table rendering the exact prediction for each sample row in `predicted_column` (`0.0` for Safe, `1.0` for Phishing) and store a local copy at `prediction_output/output.csv`.

---

## 📈 Experiment Tracking with MLflow

During model training, multiple algorithms are evaluated with hyperparameter tuning. Metrics and artifacts are logged remotely:

- **F1 Score**
- **Precision**
- **Recall**
- **Trained Model Artifacts (`model.pkl`, `preprocessor.pkl`)**

To trigger a full retraining cycle from scratch, hit:
```bash
GET http://localhost:8000/train
```

---

## 🚀 Cloud Deployment (Render / Docker)

### Deploying on Render (Free Web Service)
1. Fork or push this repository to your GitHub account.
2. Sign in to [Render](https://render.com/) and create a **New Web Service**.
3. Connect your GitHub repository and configure:
   - **Environment:** `Python 3`
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `uvicorn app:app --host 0.0.0.0 --port $PORT`
4. Click **Deploy** to receive your live public URL!

### Deploying with Docker
```bash
# Build Docker image
docker build -t networksecurity:latest .

# Run Docker container
docker run -p 8000:8000 networksecurity:latest
```

---

## 📂 Project Directory Structure

```text
├── .github/workflows/         # CI/CD deployment workflows
├── data_schema/               # YAML schema definitions for data validation
├── final_model/               # Production serialized model and preprocessor artifacts
├── networksecurity/           # Core source package
│   ├── components/            # Data Ingestion, Validation, Transformation, Model Trainer
│   ├── entity/                # Config and Artifact dataclass definitions
│   ├── logging/               # Custom timestamped logger configuration
│   ├── exception/             # Standardized custom exception handling
│   ├── pipeline/              # Training and prediction orchestrators
│   └── utils/                 # General and ML utility functions
├── templates/                 # Jinja2 HTML templates for batch prediction views
├── prediction_output/         # Generated batch inference output files
├── app.py                     # FastAPI application entrypoint
├── main.py                    # Standalone pipeline execution script
├── Dockerfile                 # Containerization definition
├── Procfile                   # Cloud process execution rule
├── requirements.txt           # Project dependencies
├── sample_test_data.csv       # Test dataset for instant inference checks
└── README.md                  # Project documentation
```

---

## 🤝 Contributing & Feedback

Contributions, feature suggestions, and issues are welcome! Feel free to fork the repository, open an issue, or submit a pull request.

⭐ **If you find this project helpful, please consider starring this repository!**