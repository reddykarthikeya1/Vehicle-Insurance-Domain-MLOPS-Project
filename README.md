# Project Vehicle Prediction Pipeline

![Python](https://img.shields.io/badge/python-3.10-blue.svg) ![MongoDB](https://img.shields.io/badge/mongodb-Atlas-green.svg) ![AWS](https://img.shields.io/badge/aws-MLOps-orange.svg)

A complete end-to-end MLOps project for vehicle data processing, training, evaluation and deployment. This repository demonstrates best practices in project structure, virtual environments, data ingestion, validation, transformation, model training/evaluation, AWS/S3 integration, CI/CD, Docker, and self-hosted GitHub runners.

---

## 🚀 Table of Contents

1. [Prerequisites](#-prerequisites)
2. [Project Setup](#-project-setup)
3. [MongoDB Atlas Configuration](#-mongodb-atlas-configuration)
4. [Logging & Exception Handling](#-logging--exception-handling)
5. [Notebook & EDA](#-notebook--eda)
6. [Data Ingestion](#-data-ingestion)
7. [Data Validation, Transformation & Model Training](#-data-validation-transformation--model-training)
8. [AWS & S3 Integration](#-aws--s3-integration)
9. [Model Evaluation & Pushing](#-model-evaluation--pushing)
10. [Prediction Pipeline & App](#-prediction-pipeline--app)
11. [CI/CD & Docker](#-cicd--docker)
12. [Self-Hosted GitHub Runner](#-self-hosted-github-runner)
13. [Usage](#-usage)
14. [License](#-license)

---

## 🎯 Prerequisites

* Python 3.10+
* `venv` module (bundled with Python)
* `pip` for package installation
* MongoDB Atlas account
* AWS account with programmatic access
* GitHub repository for CI/CD

---

## 🛠️ Project Setup

1. **Generate Template**

   ```bash
   python template.py
   ```

2. **Local Package Configuration**

   * Edit `setup.py` and `pyproject.toml` to include "src/" as a local package path.
   * For details, see [crashcourse.txt](./crashcourse.txt).

3. **Create & Activate Virtual Environment**

   ```bash
   python -m venv vehicle_env
   source vehicle_env/bin/activate   # Mac/Linux
   .\\vehicle_env\\Scripts\\activate  # Windows
   ```

4. **Install Dependencies**

   * Add required packages to `requirements.txt`.

   ```bash
   pip install -r requirements.txt
   ```

5. **Verify Installations**

   ```bash
   pip list
   ```

---

## 🗄️ MongoDB Atlas Configuration

1. Sign up for MongoDB Atlas. Create a new project and an M0 free-tier cluster.
2. Under "Database Access", create a DB user with a username & password.
3. In "Network Access", allow `0.0.0.0/0` for testing.
4. From "Clusters", click **Connect → Connect your application**. Select driver `Python` and `3.6 or later`, then copy your connection string.
5. Create a folder `notebook/` and add `mongoDB_demo.ipynb`.
6. Use the copied URI inside the notebook to push & view data.

---

## 📝 Logging & Exception Handling

* Implement `logger.py` and test it with `demo.py`.
* Build custom exceptions in `exception.py` and validate in `demo.py`.

---

## 📓 Notebook & EDA

* Add EDA & Feature Engineering notebook under `notebook/EDA_feature_engg.ipynb`.

---

## 📥 Data Ingestion

1. Define constants in `src/constants/__init__.py`.
2. Configure MongoDB connection in `src/configuration/mongo_db_connections.py`.
3. Under `src/data_access/proj1_data.py`, fetch raw data & convert to DataFrame.
4. Define entities in `src/entity/config_entity.py` and `artifact_entity.py`.
5. Implement `DataIngestion` in `src/components/data_ingestion.py`.
6. Run `demo.py` after exporting your `MONGODB_URL`:

   ```bash
   # Bash/Mac/Linux
   export MONGODB_URL="mongodb+srv://<username>:<password>@..."
   echo $MONGODB_URL

   # Windows Powershell
   $env:MONGODB_URL="mongodb+srv://<username>:<password>@..."
   echo $env:MONGODB_URL
   ```

---

## 🔄 Data Validation, Transformation & Model Training

1. Fill in `utils/main_utils.py` and configure `config/schema.yaml` for data validation rules.
2. Implement **Data Validation** component analogous to Data Ingestion.
3. Build **Data Transformation** in `src/components/data_transformation.py`, add `estimator.py` entity.
4. Develop **Model Trainer** in `src/components/model_trainer.py`, extend `estimator.py`.

---

## ☁️ AWS & S3 Integration

1. From AWS Console (region `us-east-1`), create IAM user `firstproj` with `AdministratorAccess`. Download keys.
2. Export credentials:

   ```bash
   export AWS_ACCESS_KEY_ID="<ID>"
   export AWS_SECRET_ACCESS_KEY="<SECRET>"
   ```
3. In `src/constants/__init__.py`, set:

   ```python
   MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE: float = 0.02
   MODEL_BUCKET_NAME = "my-model-mlopsproj"
   MODEL_PUSHER_S3_KEY = "model-registry"
   ```
4. Create S3 bucket `my-model-mlopsproj` (unblock public access for demo).
5. Implement S3 connection in `src/configuration/aws_connection.py` and `src/aws_storage/s3_estimator.py`.

---

## ✅ Model Evaluation & Pushing

* Build **Model Evaluation** and **Model Pusher** components in `src/components/`.

---

## 🏗️ Prediction Pipeline & App

1. Create `app.py` for inference API/CLI.
2. Add `static/` and `templates/` directories for frontend assets.

---

## 🔧 CI/CD & Docker

1. Create `Dockerfile` and `.dockerignore`.
2. Under `.github/workflows/aws.yaml`, define build, test, and deploy steps.
3. Create ECR repo `vehicleproj`. Push Docker image to ECR.

---

## 🤝 Self-Hosted GitHub Runner

1. Launch an EC2 Ubuntu 24.04 instance `vehicledata-machine`.
2. Install Docker:

   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   sudo usermod -aG docker ubuntu && newgrp docker
   ```
3. Register GitHub self-hosted runner under GitHub repository settings → Actions → Runner.

---

## 🚪 Opening Ports & Running App

1. In EC2 Security Group, add inbound rule for **Custom TCP**, port `5000`, source `0.0.0.0/0`.
2. Access the app at: `http://44.211.221.255:5000`.

## 📋 Usage

* **Training**: `http://<HOST>:5080/training`
* **Prediction**: `http://<HOST>:5080/predict?features=<comma-separated-values>`

---

## ⚖️ License

This project is licensed under the MIT License. See [LICENSE](./LICENSE) for details.
