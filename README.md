# Real-Time Fraud Detector — MLOps Project

Production-grade ML fraud detection system for banking,
fintech, and e-commerce. Scores transactions in < 100ms
with explainable AI showing WHY each transaction was flagged.

## Live Demo
- **API:** https://fraud-detector-1pje.onrender.com/docs
- **Dashboard:** https://fraud-detector-wpispku26cnqr4ui8rirjp.streamlit.app/

## Architecture
```
Transactions → Feature Store → XGBoost → FastAPI → Streamlit
```
## Tech Stack
| Layer | Tool |
|-------|------|
| ML Models | XGBoost + Random Forest + Logistic Regression |
| Imbalance Handling | SMOTE (imbalanced-learn) |
| Feature Engineering | Pandas + NumPy (30 features) |
| API Serving | FastAPI + Uvicorn |
| Containerization | Docker |
| CI/CD | GitHub Actions |
| Deployment | Render (free tier) |
| Dashboard | Streamlit Cloud |

## ML Approach
- **Type:** Supervised Learning (labeled fraud/legitimate)
- **Winner:** XGBoost (best F1 after 3-model comparison)
- **F1 Score:** 0.91+ | **ROC-AUC:** 0.98+
- **Class Imbalance:** Solved with SMOTE (2% fraud → balanced)

## Fraud Patterns Detected
| Pattern            | 				Description               |
|--------------------|----------------------------------------|
| Velocity Abuse     | 8-20 transactions per hour             |
| Geographic Anomaly | Transaction 8000km+ from home          |
| Amount Spike       | 10x-50x above user's normal spend      |
| Merchant Mismatch  | High-risk categories (crypto, jewelry) |
| Late Night Fraud   | 2am-5am transaction window             |
| Card Not Present   | Online international transactions      |

## API Endpoints
| Endpoint | Method | Description |
|----------|--------|-------------|
| /health | GET | Server health check |
| /model/info | GET | Model version + metrics |
| /predict | POST | Score single transaction |
| /predict/batch | POST | Score up to 500 transactions |
| /predict/explain | POST | Score + explain fraud signals |
| /docs | GET | Swagger UI |

## Explainable AI Example
```json
{
  "is_fraud": 1,
  "fraud_probability": 0.974,
  "risk_level": "CRITICAL",
  "decision": "BLOCK",
  "top_fraud_signals": [
    {"signal": "Geographic Anomaly", "detail": "8472km from home"},
    {"signal": "Amount Spike",       "detail": "18.4x normal spend"},
    {"signal": "Failed Attempts",    "detail": "5 failed before success"}
  ],
  "recommendation": "BLOCK transaction. Alert customer via SMS."
}
```
## Run Locally
```bash
git clone https://github.com/amikkili/fraud-detector.git
cd fraud-detector
pip install -r requirements.txt
python simulator/generate_transactions.py
python training/feature_engineering.py
python training/train_model.py
uvicorn serving.app:app --reload --port 8000
```
## Real-World Integration
In production, replace the simulator with:
- Oracle DB (core banking)
- AWS S3 (data lake)
- Kafka streams (real-time events)
- MuleSoft API (enterprise integration)

## Author
**Anil Mikkili** — MuleSoft Developer | MLOps Engineer
- LinkedIn:  linkedin.com/in/amikkili
- GitHub: https://github.com/amikkili/fraud-detector