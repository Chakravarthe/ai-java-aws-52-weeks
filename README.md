# ai-java-aws-52-weeks
# 52-Week AI Engineer Roadmap: From Java Full Stack to AI Engineer on AWS Free Tier

## The Ultimate Production‑Ready Plan

This document is your **single source of truth** for a 52‑week transformation.  
Each week you will:

1. Learn one AI/ML concept  
2. Implement it in Python (scikit‑learn / PyTorch / Transformers)  
3. Expose it via a FastAPI REST service  
4. Integrate it with a Spring Boot (Java) backend  
5. Deploy the entire stack to **AWS Free Tier** using CI/CD (GitHub Actions)  
6. Share your daily progress on LinkedIn and push all code to GitHub

By the end you will have **52 working projects** + a production‑grade capstone.

---

## 📦 Phase 0 – Foundation (Week 0)

**Goal:** Set up your environment, AWS account, GitHub repo, and first “hello world” deployment.

### Day 0 – Prerequisites

- [ ] Install VS Code + extensions: Python, Pylance, GitLens, Docker, AWS Toolkit
- [ ] Install Python 3.12, create a virtual environment (`python -m venv venv`)
- [ ] Install Java 17 (or 21) and Maven
- [ ] Install Git, create GitHub account, set up SSH keys
- [ ] Install Docker Desktop (for containerisation)
- [ ] Install AWS CLI and configure with IAM user (programmatic access)

### Day 1 – AWS Free Tier Signup

- [ ] Go to [aws.amazon.com/free](https://aws.amazon.com/free) – sign up with your credit card (you will stay within free limits)
- [ ] Set up **budget alerts** (Billing → Budgets → Create budget of $1)
- [ ] Create an IAM user with `AdministratorAccess` (for learning – restrict later in production)
- [ ] Generate access keys and configure: `aws configure`

### Day 2 – GitHub Repository & First Commit

```bash
mkdir ai-java-aws-52-weeks
cd ai-java-aws-52-weeks
git init
echo "# 52 Weeks of AI + Java + AWS" > README.md
git add .
git commit -m "Week 0: Start journey"
git remote add origin git@github.com:yourusername/ai-java-aws-52-weeks.git
git push -u origin main
```

### Day 3 – “Health Check” Spring Boot App

- [ ] Create Spring Boot project via [Spring Initializr](https://start.spring.io) with Web dependency
- [ ] Write a simple `GET /health` returning `{"status":"ok"}`
- [ ] Build: `./mvnw clean package`
- [ ] Run locally: `java -jar target/*.jar`

### Day 4 – Deploy Spring Boot to AWS Elastic Beanstalk (manual)

- [ ] Install EB CLI: `pip install awsebcli`
- [ ] Run `eb init` (choose your region, Java 17)
- [ ] Run `eb create health-api-env` (creates a t2.micro EC2 – free tier)
- [ ] Test: `eb open` → your app is live with a public URL
- [ ] **Destroy** after testing (to avoid charges) → `eb terminate health-api-env`

### Day 5 – Simple FastAPI “Hello AI”

- [ ] Create `app.py` with:

```python
from fastapi import FastAPI
app = FastAPI()
@app.get("/ping")
def ping(): return {"message": "AI service ready"}
```

- [ ] Run locally: `uvicorn app:app --reload`

### Day 6 – Dockerise FastAPI

- [ ] Write `Dockerfile`:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

- [ ] Build and run: `docker build -t ai-service . && docker run -p 8000:8000 ai-service`

### Day 7 – GitHub Actions CI (manual trigger)

- [ ] Create `.github/workflows/docker-build.yml` that builds the image on every push (no deploy yet)
- [ ] Push to GitHub → see Actions tab run

**Week 0 LinkedIn summary:**  
> Week 0 done! Environment, AWS Free Tier, first Spring Boot app deployed to Elastic Beanstalk, FastAPI containerised with Docker. Starting Week 1 tomorrow: Linear Regression with Java + AWS CI/CD. #52WeeksOfAI

---

## 🧠 Phase I – Classical Machine Learning (Weeks 1–12)

Each week follows the **same pattern** – only the AI concept changes.

### Weekly Development Procedure (for all weeks)

| Day | Activity | Output |
|-----|----------|--------|
| Mon | Understand the AI concept + generate/fetch dataset | Notebook or Python script |
| Tue | Train a baseline model (scikit‑learn) | Model file + evaluation metrics |
| Wed | Wrap model in FastAPI (single endpoint) | Local working API |
| Thu | Write Spring Boot client that calls the API | Java ↔ Python communication working |
| Fri | Containerise both services (Docker Compose) | `docker-compose up` runs everything |
| Sat | Write GitHub Actions CI/CD that deploys to AWS EC2 | Automatic deployment on push |
| Sun | Write weekly summary, tweet/LinkedIn, push final code | Public GitHub folder + post |

---

### Week 1 – Linear Regression (House Price Prediction)

#### AI Concept
Predict continuous value (price) from square footage.

#### Tasks
- Generate synthetic dataset: `sqft` → `price = 120*sqft + noise`
- Train `LinearRegression` from scikit‑learn
- Evaluate with MAE, R²

#### Java Integration
- Spring Boot endpoint `POST /predict?sqft=1500`
- Calls Python FastAPI `http://ai-service:8000/predict`
- Returns JSON with predicted price

#### AWS Deployment
- Use **EC2 t2.micro** (free tier)
- GitHub Actions:
  - Builds Docker image of FastAPI
  - Pushes to **Amazon ECR** (free 500 MB‑month)
  - SSHs into EC2, pulls image, runs container
- Spring Boot deployed to **Elastic Beanstalk** (Java environment)
- Environment variable `AI_API_URL` points to EC2 public IP

**Weekly output folder:** `week1-linear-regression/` with `README.md`, code, and screenshot.

---

### Week 2 – Logistic Regression (Spam Detection)

#### AI Concept
Binary classification, probability output.

#### Tasks
- Use SMS Spam Collection dataset (or generate)
- Train `LogisticRegression`, output probability
- Metrics: confusion matrix, precision, recall

#### Java Integration
- Java sends email text → receives probability and predicted class
- Add threshold configuration in `application.properties`

#### AWS Deployment
- Add **AWS RDS (PostgreSQL free tier)** to store prediction history
- Java writes each request/response to RDS

---

### Week 3 – Model Evaluation & Overfitting

#### AI Concept
Underfitting vs overfitting, cross‑validation.

#### Tasks
- Use `train_test_split`, compare train vs test accuracy
- Plot learning curves
- Implement k‑fold cross‑validation

#### Java Integration
- REST endpoint `/evaluate` that runs evaluation and returns metrics
- Java dashboard (simple Thymeleaf) shows metrics

#### AWS Deployment
- Store evaluation plots in **S3 bucket** (free tier 5 GB)
- Java generates pre‑signed URLs to display plots

---

### Week 4 – Feature Engineering & Scaling

#### AI Concept
One‑hot encoding, standardisation, handling missing values.

#### Tasks
- Use Titanic dataset
- Pipeline: impute → scale → encode → train

#### Java Integration
- Java sends raw passenger data (class, age, sex, etc.)
- Python applies full pipeline and returns survival probability

#### AWS Deployment
- Deploy pipeline as separate container
- Use **AWS Secrets Manager** for any API keys (free tier)

---

### Week 5 – Regularisation (Ridge / Lasso)

#### AI Concept
L1/L2 regularisation to reduce overfitting.

#### Tasks
- Compare Linear, Ridge, Lasso on Boston‑style dataset
- Show coefficient shrinkage

#### Java Integration
- Java can choose model variant via query parameter

#### AWS Deployment
- Store all three models in S3 with versioning
- Java dynamically loads model by name

---

### Week 6 – k‑Nearest Neighbours (k‑NN)

#### AI Concept
Distance‑based classification.

#### Tasks
- Iris dataset classification
- Experiment with k values

#### Java Integration
- Java sends 4 measurements → receives species

#### AWS Deployment
- Use **AWS Lambda** for k‑NN (because model is small and stateless) → cheaper than EC2

---

### Week 7 – Decision Trees & Visualisation

#### AI Concept
Rule‑based learning, interpretability.

#### Tasks
- Train on diabetes dataset
- Plot tree using `matplotlib`

#### Java Integration
- Java can request the tree image (generated on‑the‑fly)

#### AWS Deployment
- Lambda generates image, stores in S3, returns URL

---

### Week 8 – Random Forest (Ensemble)

#### AI Concept
Bagging, feature importance.

#### Tasks
- Customer churn prediction (synthetic data)
- Plot feature importance

#### Java Integration
- Java sends customer features → receives churn probability + top‑3 influential features

#### AWS Deployment
- Scheduled **EventBridge** rule triggers weekly retraining
- New model stored in S3 only if accuracy improves

---

### Week 9 – k‑Means Clustering

#### AI Concept
Unsupervised learning.

#### Tasks
- Mall customer segmentation (age, income, spending)
- Determine optimal k (elbow method)

#### Java Integration
- Java sends new customer → receives segment label (0,1,2)

#### AWS Deployment
- Use **Amazon SageMaker** free tier (250 hours of ml.t3.medium) – perfect for clustering

---

### Week 10 – PCA (Principal Component Analysis)

#### AI Concept
Dimensionality reduction.

#### Tasks
- Reduce MNIST digits from 784 to 2 dimensions
- Visualise clusters

#### Java Integration
- Java sends 784‑dim vector → receives 2D coordinates
- Java can plot on frontend chart

#### AWS Deployment
- Deploy PCA transformer as separate microservice

---

### Week 11 – Hyperparameter Tuning (GridSearchCV)

#### AI Concept
Systematic search for best parameters.

#### Tasks
- Tune a Random Forest on Heart Disease dataset
- Compare with default parameters

#### Java Integration
- Java endpoint `/trigger_tuning` starts a background job
- Job status stored in RDS

#### AWS Deployment
- Tuning runs on **AWS Batch** (free tier eligible for small jobs)
- Best model automatically promoted to production

---

### Week 12 – Bridge Project: Production ML Pipeline

**Goal:** Tie together everything from weeks 1–11 into a single production system.

**Components:**
- Feature store (PostgreSQL + S3)
- Model registry (MLflow on EC2)
- CI/CD retrains model weekly
- Java dashboard shows predictions, metrics, and drift

**Deployment:**
- Full stack on AWS Free Tier:
  - EC2 (MLflow, FastAPI)
  - RDS (PostgreSQL)
  - S3 (model artifacts)
  - Elastic Beanstalk (Spring Boot)
  - EventBridge (scheduled retraining)

**Output:** `week12-bridge/` with architecture diagram and working demo.

---

## 🧠 Phase II – Deep Learning (Weeks 13–24)

*Switch from scikit‑learn to PyTorch. Same weekly pattern, but now neural networks.*

| Week | Topic | AWS Service Focus |
|------|-------|-------------------|
| 13 | PyTorch tensors & autograd | Local only |
| 14 | Single neuron (linear regression) | EC2 (CPU) |
| 15 | MLP on MNIST | Lambda + API Gateway (serverless) |
| 16 | Optimisers (SGD, Adam) | RDS for metrics |
| 17 | Dropout & BatchNorm | EC2 + CloudWatch |
| 18 | CNN on CIFAR‑10 | ECR + EC2 (Docker) |
| 19 | Data augmentation | S3 for augmented images |
| 20 | Transfer learning (ResNet, dog breeds) | Lambda (large memory) |
| 21 | Model persistence (save/load) | S3 model registry |
| 22 | CNN feature visualisation | S3 + CloudFront |
| 23 | LSTM time series | EC2 (real‑time) |
| 24 | Bridge: image classifier serverless | Lambda + API Gateway + S3 |

**Note:** All deep learning models are trained locally (CPU). Inference only is deployed to AWS (t2.micro can handle small CNNs and LSTMs).

---

## 📚 Phase III – LLMs, RAG & Generative AI (Weeks 25–36)

*Industry‑focused – build chatbots and agents.*

| Week | Topic | AWS Service Focus |
|------|-------|-------------------|
| 25 | Text embeddings (sentence‑transformers) | OpenSearch Serverless (free) |
| 26 | Vector database (ChromaDB → OpenSearch) | OpenSearch + S3 |
| 27 | RAG from scratch (Mistral 7B locally) | EC2 t2.medium (LLM in Docker) |
| 28 | LangChain basics | ECS + EC2 |
| 29 | Prompt engineering | Secrets Manager (prompt templates) |
| 30 | LoRA fine‑tuning | SageMaker (free tier) |
| 31 | Run LLM in production (llama.cpp) | EC2 t2.medium |
| 32 | RAG evaluation (Ragas) | RDS + Lambda |
| 33 | Multi‑turn chat memory | ElastiCache (Redis) or DynamoDB |
| 34 | Function calling / tools | Lambda + API Gateway |
| 35 | OpenAI vs open source | Secrets Manager + CloudWatch |
| 36 | Bridge: production RAG chatbot | Full stack: Spring Boot + RAG service + OpenSearch + Redis |

---

## ⚙️ Phase IV – MLOps & Advanced Deployment (Weeks 37–44)

*Make your AI reliable, monitored, and automated.*

| Week | Topic | AWS Service Focus |
|------|-------|-------------------|
| 37 | Experiment tracking (MLflow) | EC2 + RDS + S3 |
| 38 | Model registry (promote staging → prod) | S3 + Lambda |
| 39 | Batch inference (scheduled) | EventBridge + Lambda |
| 40 | Real‑time inference with Docker | ECS + EC2 |
| 41 | Model monitoring (drift) | Evidently AI on Lambda + RDS |
| 42 | CI/CD for models | GitHub Actions + CodeDeploy |
| 43 | Feature store (Feast) | EC2 + RDS + S3 |
| 44 | Bridge: end‑to‑end MLOps | Complete automated pipeline |

---

## 🚀 Phase V – Advanced & Capstone (Weeks 45–52)

*Specialise and build a portfolio‑killer.*

| Week | Topic | AWS Service Focus |
|------|-------|-------------------|
| 45 | Time series (Prophet / DeepAR) | Amazon Forecast (free tier) |
| 46 | Recommendation system | EC2 + RDS |
| 47 | Object detection (YOLO) | EC2 t2.medium |
| 48 | Fine‑tune BERT for NER | Lambda (optimised) |
| 49 | AI agent with tools | EC2 + Lambda |
| 50 | Compare deployment options (Lambda vs EC2 vs SageMaker) | Three parallel deployments |
| 51 | Capstone – design & architecture | Document + cost analysis |
| 52 | Capstone – deliver & present | Fully live AI + Java + AWS product |

---

## 📁 Repository Structure (enforced each week)

```
ai-java-aws-52-weeks/
├── week0-setup/
├── week1-linear-regression/
│   ├── python-model/
│   │   ├── Dockerfile
│   │   ├── app.py
│   │   ├── model.joblib
│   │   ├── requirements.txt
│   │   └── train.py
│   ├── spring-boot-backend/
│   │   ├── src/
│   │   ├── pom.xml
│   │   └── application.properties
│   ├── .github/workflows/
│   │   └── deploy.yml
│   ├── docker-compose.yml
│   ├── README.md (weekly summary + architecture)
│   ├── linkedin-post.md
│   └── screenshot.png
├── week2-logistic-regression/
└── ...
```

---

## 🔁 Daily & Weekly Commitments

### Daily (1.5‑2 hours)
- 30 min concept & dataset
- 60 min coding (Python + Java)
- 15 min testing & local deployment
- 5 min commit + push + LinkedIn post

**LinkedIn daily post template:**
```
Day X/365 – Week Y, Day Z
✅ Built [model name] that [solves problem].
✅ Integrated with Spring Boot – Java talks to Python over REST.
✅ Deployed to AWS using [service] (still $0).
📊 Accuracy = XX%, latency = YY ms.
🔗 Code: [GitHub link]
#52WeeksOfAI #Java #AWS #MachineLearning
```

### Weekly (2‑3 hours on Sunday)
- Polish code, write `README.md`
- Create architecture diagram (draw.io / excalidraw)
- Write 500‑word article for LinkedIn / dev.to
- Test full deployment from scratch (to ensure reproducibility)

---

## 💰 Keeping AWS Costs at $0

| Service | Free Tier Limit | Your usage |
|---------|----------------|------------|
| EC2 (t2.micro) | 750 hours / month | One instance running all 52 weeks → 744 hours → ✅ |
| RDS (db.t2.micro) | 750 hours / month | One database → ✅ |
| S3 | 5 GB standard storage | Store models + images → ✅ |
| ECR | 500 MB‑month | Each model ~50 MB → ✅ |
| Lambda | 1 million requests / month | Far below |
| API Gateway | 1 million calls / month | ✅ |
| CloudFront | 1 TB data transfer | ✅ |
| EventBridge | 1 million triggers | ✅ |
| SageMaker | 250 hours of ml.t3.medium | Use only for weeks 9,30 → ✅ |
| OpenSearch Serverless | 750 hours of OCU | For weeks 25‑36 → ✅ |

**Always `eb terminate` after testing if you keep EC2 instances** – but you can keep one t2.micro running for the whole year and stay within free tier.

---

## 🧪 Example: Week 1 Step‑by‑Step (to get you started)

### Monday
- Read about linear regression (10 min)
- Run this in `python-model/train.py`:

```python
import numpy as np
from sklearn.linear_model import LinearRegression
import joblib

# Data
X = np.array([500, 800, 1200, 1500, 2000]).reshape(-1,1)
y = np.array([50, 80, 120, 150, 200])

# Train
model = LinearRegression()
model.fit(X, y)

# Save
joblib.dump(model, 'model.joblib')
```

### Tuesday
- Wrap model in FastAPI (`app.py`): endpoint `/predict`

### Wednesday
- Create Spring Boot `PredictionController` that calls `http://localhost:8000/predict`

### Thursday
- Run both services manually → test with curl / Postman

### Friday
- Write `Dockerfile` and `docker-compose.yml`

### Saturday
- Write `.github/workflows/deploy.yml` that:
  - Logs into AWS ECR
  - Builds and pushes Docker image
  - Restarts EC2 instance with new image

### Sunday
- Write README, diagram, post to LinkedIn, push to GitHub

---

## ✅ Final Checklist Before Week 1 Starts

- [ ] AWS account created, budget alert active
- [ ] GitHub repository `ai-java-aws-52-weeks` cloned locally
- [ ] `venv` created and activated
- [ ] All extensions installed (Python, Java, Docker, AWS)
- [ ] First Spring Boot “Hello World” deployed manually to Elastic Beanstalk
- [ ] Docker desktop working
- [ ] GitHub Actions secret `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` stored

---

## 🎯 Success Metrics (After 52 Weeks)

| Metric | Target |
|--------|--------|
| GitHub contributions | 365+ days of green squares |
| LinkedIn followers | 2000+ from the AI/Java community |
| Projects deployed | 52 (all live on AWS for demo) |
| Job interviews for AI Engineer | At least 3 reach‑outs |
| Code reusability | Any week can be cloned and run in 15 minutes |

---

## 📢 Final Word

This is not a passive course – it is an **active building journey**.  
You will fail (models will underfit, deployments will crash, AWS bills will scare you). That is exactly how you learn.

**Your only job today:** Complete Phase 0 (Week 0). Then start Week 1 – Monday.

I will be here to answer questions every step of the way. Now go build. 🚀

