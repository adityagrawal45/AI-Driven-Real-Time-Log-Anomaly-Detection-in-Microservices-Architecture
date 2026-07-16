🛡️ WatchTower
AI-Powered Real-Time Anomaly Detection for Microservices
<p align="center"> <img src="https://img.shields.io/badge/Java-17-orange?style=for-the-badge&logo=openjdk"/> <img src="https://img.shields.io/badge/Spring_Boot-3.x-success?style=for-the-badge&logo=springboot"/> <img src="https://img.shields.io/badge/Python-3.14-blue?style=for-the-badge&logo=python"/> <img src="https://img.shields.io/badge/Apache-Kafka-black?style=for-the-badge&logo=apachekafka"/> <img src="https://img.shields.io/badge/Redis-red?style=for-the-badge&logo=redis"/> <img src="https://img.shields.io/badge/Docker-Compose-blue?style=for-the-badge&logo=docker"/> <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge"/> </p> <p align="center">
⚡ Detect anomalies before your users do.

WatchTower is an intelligent, event-driven microservices platform that continuously monitors distributed systems and identifies abnormal behavior in milliseconds using Machine Learning.

Instead of waiting for outages, WatchTower predicts them.

🎥 Demo

Imagine a production environment where thousands of events flow every second.

Microservice Logs
        │
        ▼
 Apache Kafka
        │
        ▼
 Log Consumer
        │
        ▼
 AI Detection Engine
(Isolation Forest)
        │
        ▼
 Redis Cache
        │
        ▼
 REST API
        │
        ▼
 Dashboard / Monitoring Tools
🌟 Why WatchTower?

Modern applications generate millions of logs every day.

Finding one problematic service among hundreds is like finding a needle in a haystack.

WatchTower automates this process using Artificial Intelligence.

Instead of manually inspecting logs, it:

✅ Collects telemetry

✅ Learns normal behaviour

✅ Detects anomalies in real-time

✅ Stores predictions instantly

✅ Exposes everything through REST APIs

🚀 Features

✨ Real-Time AI Anomaly Detection

📡 Kafka Event Streaming

🧠 Online Isolation Forest Learning

⚡ Redis Low-Latency Storage

🌍 Spring Cloud Gateway

🔍 Eureka Service Discovery

📦 Dockerized Infrastructure

📊 Experiment Reproduction Pipeline

📈 Performance Benchmarking

🛠 Rule-Based Fallback Engine

🏗 System Architecture
                   +----------------------+
                   |   Log Producer       |
                   |  Spring Boot         |
                   +----------+-----------+
                              |
                              |
                     Apache Kafka
                              |
                              ▼
                   +----------------------+
                   |   Log Consumer       |
                   +----------+-----------+
                              |
                              ▼
                   +----------------------+
                   | AI Detection Service |
                   | Python + Flask       |
                   | Isolation Forest     |
                   +----------+-----------+
                              |
                    Prediction Results
                              |
                              ▼
                        Redis Cache
                              |
                              ▼
                  +----------------------+
                  |  Anomaly REST API    |
                  +----------+-----------+
                              |
                       API Gateway
                              |
                              ▼
                        REST Clients
🧠 AI Engine

The intelligence behind WatchTower is powered by

Isolation Forest
Online Learning
Dynamic Thresholding
Rule-Based Cold Start Detection

Input Features

Latency

CPU Usage

Memory Usage

User Count

Error Rate

Output

Anomaly Score

True / False

Timestamp

Average inference latency

⚡ 8.83 ms
🏢 Tech Stack
Category	Technologies
Backend	Java 17, Spring Boot
AI	Python, Flask, Scikit-Learn
Messaging	Apache Kafka
Database	Redis
Cloud	Spring Cloud Gateway
Discovery	Eureka
Containerization	Docker
Build	Maven
ML	Isolation Forest
Evaluation	Pandas, NumPy, Matplotlib
📂 Project Structure
WatchTower
│
├── ai-service
├── anomaly-api
├── api-gateway
├── eureka-server
├── kafka
├── log-consumer
├── log-producer
├── scripts
├── docs
├── docker-compose.yml
├── requirements.txt
└── README.md
⚡ Quick Start

Clone

git clone https://github.com/yourusername/watchtower.git

cd watchtower

Start Everything

docker compose up --build

Open

http://localhost:8761

Verify

AI Service

API Gateway

Redis

Kafka

Spring Boot Services

Everything starts automatically.

🔥 Workflow
Application

↓

Generate Logs

↓

Kafka Topic

↓

Consumer

↓

AI Prediction

↓

Redis

↓

REST API

↓

Monitoring Dashboard
📊 Performance
Metric	Result
Average Detection Latency	8.83 ms
Streaming	Real-Time
Cache	Redis
Architecture	Event Driven
Deployment	Docker
📸 Screenshots
📷 Add

• Eureka Dashboard

• Docker Containers

• Kafka UI

• API Responses

• Redis Insight

• Terminal Demo

• Architecture Diagram
🎯 Future Roadmap
Kubernetes Deployment
Prometheus Metrics
Grafana Dashboard
OpenTelemetry Integration
ELK Stack
Jaeger Distributed Tracing
Web Dashboard
Explainable AI
LSTM-based Detection
Auto Scaling
🤝 Contributing

Pull Requests are welcome.

If you'd like to improve WatchTower:

Fork

Create Feature Branch

Commit Changes

Push

Open Pull Request
📚 Research

This repository accompanies the paper

WatchTower: A Real-Time Anomaly Detection Framework for Microservice-Based Applications

⭐ If you like this project

Give it a ⭐

It motivates future development.
