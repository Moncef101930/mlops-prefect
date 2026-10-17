# MLOps Project — Customer Churn Prediction

## 1. Project Overview

This project implements an end-to-end MLOps workflow for customer churn prediction using machine learning, pipeline orchestration, automated testing, code quality analysis, security checks, and REST API deployment.

The objective is to organize the machine learning lifecycle into reproducible steps, from data preparation and model training to model evaluation and API exposure.

## 2. Objectives

- Prepare and preprocess customer churn data.
- Train a machine learning classification model.
- Save and load the trained model.
- Evaluate model performance.
- Orchestrate ML workflows using Prefect.
- Automate code formatting and quality checks.
- Perform security analysis using Bandit.
- Execute unit tests using Pytest.
- Expose predictions through a FastAPI REST API.
- Document each implementation stage with screenshots.

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Pandas | Data manipulation |
| Scikit-learn | Machine learning and preprocessing |
| Joblib | Model serialization |
| Prefect | Workflow orchestration |
| FastAPI | REST API development |
| Uvicorn | ASGI server |
| Black | Code formatting |
| Pylint | Code quality analysis |
| Bandit | Python security analysis |
| Pytest | Automated testing |
| Git | Version control |
| GitHub | Source code and technical documentation |

## 4. Project Structure

```text
ml_project/
├── main.py
├── model_pipeline.py
├── pipeline_prefect.py
├── app.py
├── Churn_Modelling.csv
├── churn_model.pkl
├── requirements.txt
├── fastapi.log
├── tests/
└── journalisation/
    ├── README.md
    └── screenshots/
```

*Note: This is the intended project structure. Some files and folders may not exist yet in your local repository.*

## 5. Machine Learning Workflow

The machine learning pipeline consists of the following stages:

1. **Data preparation:** load the dataset, remove unnecessary columns, encode categorical variables, and prepare the training and testing sets.
2. **Model training:** train a classification model using Scikit-learn.
3. **Model persistence:** save the trained model for later use.
4. **Model loading:** reload the saved model when required.
5. **Model evaluation:** evaluate predictions on test data.

The project uses a Linear Support Vector Classification model (`LinearSVC`) for the customer churn classification task.

## 6. Prefect Workflow Orchestration

Prefect is used to organize the machine learning process into tasks and flows.

### Available flows

| Flow | Description |
|---|---|
| `code` | Runs code formatting, quality analysis, security checks, and unit tests |
| `all` | Executes code validation followed by the ML pipeline |
| `entrainement` | Executes data preparation and model training |
| `evaluate` | Loads the saved model |
| `api` | Starts the FastAPI service |

### Execute the code validation flow

```bash
python pipeline_prefect.py --flow code
```

### Execute the complete pipeline

```bash
python pipeline_prefect.py --flow all
```

### Execute model training

```bash
python pipeline_prefect.py --flow entrainement
```

### Execute the evaluation flow

```bash
python pipeline_prefect.py --flow evaluate
```

### Start the API through Prefect

```bash
python pipeline_prefect.py --flow api
```

## 7. Code Quality and Security

The project integrates tools to improve code reliability and maintainability.

### Black

Black formats Python source files automatically.

### Pylint

Pylint analyzes source code and reports potential quality issues.

### Bandit

Bandit checks Python source code for common security problems.

### Pytest

Pytest executes automated tests to verify the behavior of the project.

These checks are integrated into the Prefect `code` flow.

## 8. FastAPI REST API

FastAPI exposes the trained churn prediction model through HTTP endpoints.

### Start the API directly

```bash
python -m uvicorn app:app --host 127.0.0.1 --port 8000
```

Alternatively, launch it through Prefect:

```bash
python pipeline_prefect.py --flow api
```

### Available endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/` | API welcome message |
| GET | `/health` | Check API availability |
| POST | `/predict` | Predict customer churn |

The actual available endpoints depend on the implementation in `app.py`.

### Interactive API documentation

Once the server is running, open:

http://127.0.0.1:8000/docs

### Health check

```bash
curl http://127.0.0.1:8000/health
```

The endpoint should return a successful response when the API is running.

### Example prediction request

Example JSON payload for the `/predict` endpoint:

```json
{
  "CreditScore": 650,
  "Geography": "France",
  "Gender": "Male",
  "Age": 40,
  "Tenure": 5,
  "Balance": 75000,
  "NumOfProducts": 2,
  "HasCrCard": 1,
  "IsActiveMember": 1,
  "EstimatedSalary": 60000
}
```

The response should indicate the predicted class and whether the customer is predicted to churn, according to the API implementation.

## 9. Installation and Execution

### Step 1 — Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_REPOSITORY_NAME>
```

Replace the placeholders with your actual GitHub repository URL and directory name.

### Step 2 — Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Linux or WSL:

```bash
source .venv/bin/activate
```

### Step 3 — Install dependencies

```bash
python -m pip install -r requirements.txt
```

Ensure that the required dependencies are listed in `requirements.txt`, including Prefect, FastAPI, Uvicorn, Pandas, Scikit-learn, Joblib, Black, Pylint, Bandit, and Pytest as applicable.

### Step 4 — Check the environment

```bash
python --version
python -m pip list
```

### Step 5 — Execute a workflow

For example:

```bash
python pipeline_prefect.py --flow api
```

## 10. Work Journal and Screenshots

This section documents the practical implementation of the MLOps workshop.

### Step 1 — Project setup and dependencies

**Objective:** Prepare the Python environment and install the project dependencies.

**Evidence:**
- Environment setup.
- Installed dependencies.
- Project structure.

### Step 2 — Machine learning pipeline

**Objective:** Prepare the data, train the model, save it, and evaluate its performance.

**Evidence:**
- Data preparation and training output.
- Model evaluation results.
- Saved model artifact.

### Step 3 — Prefect orchestration and code validation

**Objective:** Organize tasks into Prefect flows and integrate automated code checks.

**Evidence:**
- Prefect flow execution.
- Black formatting output.
- Pylint analysis.
- Bandit security analysis.
- Pytest results.

### Step 4 — FastAPI integration with Prefect

**Objective:** Add a dedicated Prefect task and flow to launch the FastAPI service.

**Implementation:**
- Added `launch_api_task`.
- Added `api_flow`.
- Added `api` to the command-line flow choices.
- Configured Uvicorn to launch the API in the background.
- Added a health check and log file for startup diagnostics.

**Execution command:**

```bash
python pipeline_prefect.py --flow api
```

**Verification commands:**

```bash
curl http://127.0.0.1:8000/health
```

Open the interactive documentation:

http://127.0.0.1:8000/docs

**Screenshots:**

Add your screenshots to `journalisation/screenshots/` and insert them below.

#### 4.1 Prefect API flow

![Prefect API flow execution](screenshots/etape-04-prefect-api.png)

#### 4.2 FastAPI Swagger documentation

![FastAPI Swagger documentation](screenshots/etape-04-swagger.png)

#### 4.3 API health check

![FastAPI health check](screenshots/etape-04-health.png)

## 11. Expected Results

At completion, the project should provide:

- A reproducible customer churn classification workflow.
- Prefect flows for model processing and code validation.
- Automated formatting, quality, security, and testing steps.
- A REST API exposing model predictions.
- Technical documentation and screenshots documenting the implementation.

Actual model metrics and test outcomes should be recorded after running the relevant workflows.

## 12. Future Improvements

Possible improvements include:

- Persisting the preprocessing scaler together with the trained model.
- Adding input validation and consistent categorical encoding.
- Improving automated test coverage for the prediction endpoint.
- Containerizing the application with Docker.
- Adding CI/CD automation.
- Monitoring API availability and prediction behavior.
- Implementing a model retraining endpoint with appropriate authentication and access controls.

## 13. Author

**Moncef Ben Maatallah**

MLOps | Machine Learning | Data Analytics | Python

This repository documents practical work on machine learning pipeline automation, workflow orchestration, software quality, and API deployment.
