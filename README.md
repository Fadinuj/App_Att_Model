# 🚦 Radcom Encrypted Traffic Analysis System

A production-grade **Machine Learning System** designed to classify encrypted network traffic. This project was developed for the **Radcom 2026 Competition** and tackles two major challenges: **Application Identification** (128 classes) and **Attribution Classification** (5 types).

The system is containerized using **Docker** and follows a microservices architecture to ensure scalability, asynchronous processing, and ease of deployment.

> ⚠️ **Data Privacy Notice:**
> The training and validation datasets (CSV files) are **not included** in this repository due to permission and privacy restrictions. The system logic, architecture, and training scripts are fully available, but users must provide their own datasets formatted according to the project specifications to run the training pipelines.

---

## 🧠 Challenges Solved

### 1. Application Identification
* **Goal:** Classify encrypted traffic flows into **128 distinct applications** (e.g., WhatsApp, Netflix, Zoom, Teams).
* **Input:** Statistical network flow features (Packet sizes, Inter-arrival times, Flow duration, TCP flags, etc.).
* **Approach:** Custom Machine Learning model optimized for high-dimensional, imbalanced data.

### 2. Attribution Identification
* **Goal:** Classify the *category* of traffic usage into **5 types**:
    * Streaming
    * File Transfer
    * VoIP / Real-time
    * Browsing
    * Chat / Messaging

---

## 🏗️ System Architecture

The project implements a modern **MLOps** pipeline using **Docker Compose**. It separates the frontend, backend, and heavy computational tasks.

```mermaid
graph LR
    User([User]) -- "Upload CSV" --> UI[Streamlit UI]
    UI -- "REST API Request" --> API[FastAPI Backend]
    API -- "Enqueue Task" --> Redis[(Redis Broker)]
    Redis -- "Consume Task" --> Worker[Celery Worker]
    Worker -- "Run Inference" --> Model[ML Model]
    Worker -- "Save Prediction" --> Redis
    UI -.-> "Poll Results" --> Redis
    Redis -- "Return CSV" --> UI

📂 Project Structure
├── App/
│   ├── train_app.py           # Training script for Application Model (128 classes)
│   ├── train_att.py           # Training script for Attribution Model (5 classes)
│   ├── common_app.py          # Shared utility functions (Preprocessing, Feature Eng.)
│   └── traffic-system/        # Microservices Deployment Package
│       ├── docker-compose.yml # Orchestration configuration
│       ├── api/               # API Service (FastAPI)
│       │   ├── main.py
│       │   └── Dockerfile
│       ├── ui/                # Frontend Service (Streamlit)
│       │   ├── app.py
│       │   └── Dockerfile
│       └── worker/            # Background Worker (Celery)
├── Data/                      # (Empty - Data not included due to restrictions)
└── README.md
