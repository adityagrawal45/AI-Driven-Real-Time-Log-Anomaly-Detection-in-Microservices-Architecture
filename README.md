WatchTower — Real-Time Anomaly Detection for Microservices
WatchTower is a production-ready, event-driven microservices platform that detects anomalous behaviour in distributed systems using an online Isolation Forest model. Structured telemetry (latency, error rate, CPU, memory, user load) is streamed through Apache Kafka, scored in real time by a Python/Flask AI service, and stored in Redis for low-latency retrieval via a Spring Boot REST API.

This repository accompanies the manuscript:

"WatchTower: A Real-Time Anomaly Detection Framework for Microservice-Based Applications" [submitted for journal review — 2025]

Features
Real-time anomaly scoring via Isolation Forest (≤ 8.83 ms mean inference latency)
Kafka-based asynchronous log ingestion — decoupled from detection
Rule-based fallback detector active before the model is trained
Redis result caching for instant API retrieval
Spring Cloud (Eureka + API Gateway) for service discovery and routing
Fully containerised: one docker-compose up starts the entire stack
Reproducible evaluation pipeline producing all manuscript figures and tables
Architecture
┌──────────────┐   Kafka    ┌──────────────┐   HTTP    ┌──────────────┐
│ Log Producer │──────────►│ Log Consumer │──────────►│  AI Service  │
│ (Spring Boot)│           │ (Spring Boot)│           │  (Flask/IF)  │
└──────────────┘           └──────┬───────┘           └──────────────┘
                                  │ Redis
                           ┌──────▼───────┐
                           │ Anomaly API  │◄── REST clients
                           │ (Spring Boot)│
                           └──────────────┘
                                  ▲
                           ┌──────┴───────┐
                           │ API Gateway  │
                           │(Spring Cloud)│
                           └──────────────┘
                           ┌──────────────┐
                           │Eureka Server │ (service registry)
                           └──────────────┘
Services
Service	Language	Port	Role
log-producer	Java / Spring Boot	8080	Generates synthetic log events and publishes to Kafka
log-consumer	Java / Spring Boot	8082	Consumes Kafka events, invokes AI service, stores results
ai-service	Python / Flask	5000	Trains Isolation Forest, exposes /detect-anomalies
anomaly-api	Java / Spring Boot	8081	Reads anomaly results from Redis, exposes REST API
eureka-server	Java / Spring Boot	8761	Netflix Eureka service registry
api-gateway	Java / Spring Cloud	8085	Single entry point; routes to all backend services
Infrastructure: Apache Kafka (port 9092), Zookeeper (2181), Redis (6379).

Tech Stack
Layer	Technology
Message broker	Apache Kafka 7.4 (Confluent)
Cache / store	Redis 7
AI / ML	Python 3.14, Flask, scikit-learn (Isolation Forest)
Microservices	Java 17, Spring Boot 3, Spring Cloud
Service discovery	Netflix Eureka
API routing	Spring Cloud Gateway
Containerisation	Docker, Docker Compose
Evaluation	Python: pandas, numpy, scipy, matplotlib
Prerequisites
Tool	Minimum version	Notes
Docker Desktop	24	Required for docker-compose
Docker Compose	2.20	Bundled with Docker Desktop
Java JDK	17	Only needed to build services outside Docker
Maven	3.9	Or use the included ./mvnw wrapper
Python	3.10+	Only needed to run evaluation scripts
Installation
git clone https://github.com/hrsvd/microservices-anomaly-detection.git
cd microservices-anomaly-detection
Install Python evaluation dependencies (optional — only for running experiments):

pip install -r requirements.txt
Running with Docker (Recommended)
This is the simplest way to start the full stack.

docker-compose up --build
All eight containers start in dependency order. The Kafka broker, Eureka registry, and Redis must be healthy before the application services start.

Verify the stack is running:

http://localhost:8761          # Eureka dashboard — all services should appear
http://localhost:5000/health   # AI service health check
http://localhost:8081/anomalies # Anomaly results API
http://localhost:8085          # API Gateway entry point
Stop the stack:

docker-compose down
Stop and remove all data volumes:

docker-compose down -v
Running Locally (without Docker)
1. Start infrastructure
# Kafka + Zookeeper + Redis (requires Docker)
docker-compose up -d zookeeper kafka redis

# Create Kafka topics
bash kafka/create-topics.sh
2. Start the AI service
cd ai-service
pip install -r requirements.txt
python app.py
3. Start Spring Boot services (in separate terminals)
# Eureka Server (start first)
cd eureka-server && ./mvnw spring-boot:run

# Log Producer
cd log-producer && ./mvnw spring-boot:run

# Log Consumer
cd log-consumer && ./mvnw spring-boot:run

# Anomaly API
cd anomaly-api && ./mvnw spring-boot:run

# API Gateway
cd api-gateway && ./mvnw spring-boot:run
Configuration
Each Spring Boot service is configured via src/main/resources/application.yml.

Key environment variables (also accepted by Docker Compose):

Variable	Default	Used by
SPRING_KAFKA_BOOTSTRAP_SERVERS	kafka:9092	log-producer, log-consumer
REDIS_HOST	redis	log-consumer, anomaly-api
AI_SERVICE_URL	http://ai-service:5000	log-consumer
EUREKA_SERVER	http://eureka-server:8761/eureka/	ai-service
EUREKA_CLIENT_SERVICEURL_DEFAULTZONE	http://eureka-server:8761/eureka/	api-gateway
API Documentation
AI Service (http://localhost:5000)
POST /detect-anomalies
Accepts a JSON array of log records and returns anomaly scores.

Request body:

[
  {
    "serviceName": "OrderService",
    "latency": 620,
    "errorRate": 0.02,
    "userCount": 150,
    "memoryUsage": 72.4,
    "cpuUsage": 58.1
  }
]
Response:

[
  {
    "serviceName": "OrderService",
    "anomalyScore": 0.847,
    "anomaly": true,
    "timestamp": "2025-07-10T14:32:01.123"
  }
]
GET /health
Returns model status and readiness.

Anomaly API (http://localhost:8081)
GET /anomalies
Returns all anomaly results stored in Redis.

Running Experiments
The scripts/ directory contains the full reproducibility pipeline for the manuscript.

# Run the complete evaluation pipeline (generates all figures and tables)
python scripts/run_experiments.py

# Regenerate figures only (requires results/ to exist)
python scripts/generate_figures.py
See EXPERIMENTS.md for a step-by-step guide, expected outputs, and which manuscript sections each script supports.

Reproducing Paper Results
All quantitative results in the manuscript are reproducible from the pipeline above. Pre-computed outputs are stored in results/:

File	Contents	Manuscript section
results/baseline_results.json	IF vs LOF vs OC-SVM vs Rule-Based metrics	Table II
results/multi_run_results.json	10-seed mean ± std + paired t-test	Section 8.3
results/threshold_report.json	Youden's J optimal threshold (0.3903)	Section 5.4
results/sweep_results.csv	4×5 hyperparameter grid (n_estimators × contamination)	Section 6.4
results/latency_report.json	Mean inference latency (8.83 ms)	Section 7.1
Publication figures are in figures/ (Figures 2–6 of the manuscript).

Project Structure
microservices-anomaly-detection/
├── ai-service/               Python Flask service — Isolation Forest model
├── anomaly-api/              Spring Boot REST API — reads anomaly results
├── api-gateway/              Spring Cloud Gateway — single entry point
├── eureka-server/            Netflix Eureka service registry
├── log-consumer/             Spring Boot Kafka consumer — calls AI service
├── log-producer/             Spring Boot Kafka producer — synthetic log generator
├── kafka/                    Kafka topic setup scripts
├── scripts/                  Python evaluation pipeline
│   ├── run_experiments.py    Master pipeline (runs all steps)
│   ├── baselines.py          Baseline comparisons (IF/LOF/OC-SVM/Rule-Based)
│   ├── derive_threshold.py   ROC/PR threshold derivation
│   ├── generate_data.py      Synthetic dataset generator
│   ├── generate_figures.py   Regenerates all publication figures
│   ├── hyperparameter_sweep.py  n_estimators × contamination grid search
│   ├── loghub_adapter.py     Maps Loghub public datasets to feature schema
│   └── multi_run.py          10-seed multi-run + paired t-test
├── results/                  Experiment outputs (JSON/CSV metric files)
├── docs/                     Submission checklist and submission guide
├── docker-compose.yml        Full-stack container orchestration
├── requirements.txt          Python evaluation dependencies
├── README.md                 This file
├── EXPERIMENTS.md            Experiment reproduction guide
Troubleshooting
Kafka not ready / log-consumer exits immediately Kafka takes 30–60 seconds to initialise. On slow machines the consumer may start before the broker is ready. Re-run docker-compose up or add a manual delay.

AI service not trained yet The model requires at least 50 log samples before training. The service falls back to rule-based detection automatically until then.

Services not appearing in Eureka Check http://localhost:8761. Services register on startup and may take 30 seconds to appear. Verify the EUREKA_SERVER environment variable is correct.

Python script: ModuleNotFoundError Install dependencies: pip install -r requirements.txt. Always run scripts from the project root: python scripts/run_experiments.py.

results/logs.csv not found Run python scripts/run_experiments.py first to generate the dataset and all intermediate results before running generate_figures.py standalone.

License
This project is licensed under the MIT License. See LICENSE for details.

Citation
If you use WatchTower or the accompanying evaluation pipeline in your research, please cite:

@misc{kushwah2026watchtower,
  author = {Harshvardhan Kushwah},
  title  = {WatchTower: A Real-Time Anomaly Detection Framework for Microservice-Based Applications},
  year   = {2026},
  note   = {Under review}
Contributing
Contributions are welcome. Please open an issue before submitting a pull request.
