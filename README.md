# Bastion: Transformer-Based End-to-End Web Application Firewall (WAF) Pipeline

## Problem Statement:

Traditional Web Application Firewalls (WAFs) primarily utilize rule-based detection such as ModSecurity. While effective against known attack patterns, they struggle to identify zero-day exploits and novel anomalies due to the limitations of static rulesets. Transformer models, with their capacity to learn complex patterns from raw web traffic, present a powerful alternative. By leveraging Transformers, a WAF can potentially detect anomalous requests without reliance on hard-coded signatures, thereby enhancing protection against unknown threats.

**## Key Deliverables:**

* End-to-end log processing pipeline 
* Transformer-based anomaly detection model 
* Real-time integration with Apache/Nginx 
* Non-blocking, high-throughput inference engine 
* Automated continuous learning framework 

## Data Flow

1.  **Log Collection:** Apache/Nginx logs → Ingestion Module (batch or streaming) 
2.  **Parsing:** Raw logs → Structured key-value pairs with normalization 
3.  **Tokenization:** Normalized requests → Token sequences 
4.  **Inference:** Token sequences → Transformer model → Anomaly scores 
5.  **Detection:** Anomaly scores → Threshold comparison → Action (log/alert/block) 
6.  **Retraining:** New benign traffic → Incremental model update 

**## Key advantages of this approach:**

* **Adaptive Learning:** Continuous improvement from new traffic patterns 
* **Zero-Day Protection:** Detection of previously unseen attack patterns 
* **Non-Blocking Architecture:** Real-time inference without disrupting legitimate traffic 
* **Scalability:** Batch and streaming processing for various deployment scenarios 
* **Maintainability:** Clean separation of concerns and modular design 

## Conclusion

This comprehensive solution provides a production-ready Transformer-based Web Application Firewall that combines the power of deep learning with practical deployment considerations. By following this architecture and implementation guide, teams can build a WAF system that learns from benign traffic patterns and detects novel anomalies in real time without the limitations of static rule-based systems.


## Repo structure:
```bash
bastion/
├── .github/ #CI/CD workflows for automated testing, building and deployment 
│   └── workflows/
│       ├── ci-pipeline.yml # Runs linting, vulnerability scanning, and unit tests on every push.
│       └── cd-pipeline.yml # Builds Docker images and deploys services via Kubernetes.
├── .dockerignore
├── .gitignore
├── README.md
├── requirements.txt
│
├── config/  # centralized environments-specific configuaration files
│   ├── default.py
│   ├── production.py # for cloud deployment settings
│   └── development.py # for local development overrides
│
├── data/ # sample log files for quick testing and local validation, will help with environment for debugging
│   ├── sample_logs_benign.log
│   └── sample_logs_malicious.log
│
├── docs/ # documentation
│   ├── architecture.md # system overview and data flow
│   ├── api.md # api specs for inference service
│   └── setup_guide.md # env setup and local deployment instructions
│
│ #ignore infra for now
├── infrastructure/ #Infrastructure as code (IaC) definitions
│   ├── terraform/ #cloud resource provisioning
│   └── kubernetes/ # deployment manifests for each service
│       ├── inference-deployment.yml
│       └── training-cronjob.yml
│
├── notebooks/ # notebooks for expermentation and prototyping
│   ├── 1_data_exploration.ipynb # for anyalyzing traffic patterns
│   └── 2_model_prototyping.ipynb # for new ml architectures before prod integration
│
├── src/
│   └── bastion/
│       ├── __init__.py
│       │
│       ├── common/ # shared utilities (structured logging, pydantic schemes)
│       │   ├── __init__.py
│       │   ├── logging.py
│       │   └── schemas.py
│       │
│       ├── ingestor/ # lightweight agent that tails web server logs and pushes them to a message queue
│       │   ├── __init__.py
│       │   └── agent.py
│       │
│       ├── preprocessor/ # parses raw log strings & tokenizes them for ML inference
│       │   ├── __init__.py
│       │   ├── parser.py
│       │   └── tokenizer.py
│       │
│       ├── inference/ # runs a transformer-based anomaly detection model on incoming requests
│       │   ├── __init__.py
│       │   ├── main.py
│       │   ├── model.py
│       │   └── Dockerfile
│       │
│       ├── decision_engine/ # applies detection logic and triggers action
│       │   ├── __init__.py
│       │   ├── engine.py
│       │   └── Dockerfile
│       │
│       └── training/ # batch pipeline for retraining and feedback-based continuous learning
│           ├── __init__.py
│           ├── pipeline.py
│           └── feedback_loop.py
│
└── tests/ # automated test suite mirroring the src folder
    ├── __init__.py
    ├── integration/ # end-to-end flow validation
    │   └── test_full_pipeline.py
    └── unit/ # function level tests
        ├── test_preprocessor.py
        └── test_decision_engine.py
```

## Component structure
| Step | Component                     | Description                                                |
| ---- | ----------------------------- | ---------------------------------------------------------- |
| 1️  | **ingestor/**                 | Collects and queues raw web logs.                          |
| 2️  | **preprocessor/parser.py**    | Parses raw log lines into structured data.                 |
| 3️  | **preprocessor/tokenizer.py** | Converts parsed logs into ML-ready token sequences.        |
| 4️  | **inference/main.py**         | Runs the model to produce anomaly scores.                  |
| 5️  | **decision_engine/engine.py** | Applies threshold-based logic and triggers alerts/actions. |
| 6  | **training/pipeline.py**      | Retrains model using newly collected benign traffic.       |



**Creators:** Rishita Sharma, Mitish Raina, Paarth Sharma 