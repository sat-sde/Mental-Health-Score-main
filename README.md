# Mental Health Signal (Mansik Santulan Score)

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100%2B-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.3%2B-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![Deploy to Render](https://img.shields.io/badge/Render-Ready-46E3B7?logo=render&logoColor=white)](https://render.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An end-to-end Machine Learning web application designed to evaluate and predict a student's mental health score based on digital habits, sleep cycles, physical activity, academic demands, and stress levels. 

The project features a complete machine learning workflow: from exploratory data analysis and feature pipeline engineering to a high-performance **FastAPI** backend and a bespoke, responsive **Frontend UI** with dynamic animated gauge telemetry.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Project Architecture](#project-architecture)
- [Machine Learning Pipeline](#machine-learning-pipeline)
  - [Input Features](#input-features)
  - [Preprocessing & Modeling](#preprocessing--modeling)
  - [Score Interpretation](#score-interpretation)
- [API Reference](#api-reference)
  - [Endpoints](#endpoints)
  - [Example Request & Response](#example-request--response)
- [Local Development Setup](#local-development-setup)
- [Deployment on Render](#deployment-on-render)
- [Technology Stack](#technology-stack)
- [Disclaimer](#disclaimer)

---

## Overview

Modern students face unique challenges balancing academic deadlines, screen time, social media engagement, and restorative habits. **Mental Health Signal** provides an algorithmic wellness baseline based on behavioral patterns rather than clinical diagnostics.

By analyzing key lifestyle indicators (daily screen unlocks, social media hours, sleep duration, exercise, and perceived stress), the trained regression pipeline computes a continuous mental wellness score on a scale from **0.00 to 10.00**.

---

## Key Features

- **Trained Machine Learning Model**: Production-ready Scikit-Learn regression pipeline (`Mental_Health_Model.pkl`) trained on empirical student lifestyle data.
- **Unified FastAPI Web Service**: High-speed asynchronous Python server that serves both the REST API and the frontend client assets from a single endpoint.
- **Dynamic Frontend Dashboard**:
  - Bespoke dark-mode aesthetic with custom typography (*Fraunces*, *Inter*, *JetBrains Mono*).
  - SVG semi-circular gauge displaying animated score readings.
  - Granular client-side input validation and server-side Pydantic error mapping.
- **Zero-Config Cloud Deployment**: Pre-configured `render.yaml` for 1-click Render Blueprint deployment.
- **Interactive Documentation**: Automatic OpenAPI/Swagger UI available out-of-the-box at `/docs`.

---

## Project Architecture

```plaintext
Mental-Health-Score-main/
│
├── main.py                                      # FastAPI application and route handlers
├── Mental_Health_Model.pkl                      # Serialized Scikit-learn ML pipeline
├── ML_Project.ipynb                             # Jupyter Notebook: EDA, training & evaluation
├── ML Project.html                              # Exported interactive notebook report
├── Student Social Media And Mental Health...csv # Training dataset
│
├── index.html                                   # Main frontend user interface
├── style.css                                    # Custom styles, animations, and design tokens
├── script.js                                    # Form handling, validation, and gauge animation
│
├── requirements.txt                             # Python dependencies
├── render.yaml                                  # Render Blueprint deployment configuration
├── .gitignore                                   # Git ignore rules for Python & IDEs
└── README.md                                    # Project documentation
```

---

## Machine Learning Pipeline

### Input Features

The model analyzes 12 behavioral and demographic attributes:

| Feature | Type | Valid Values / Range | Description |
| :--- | :--- | :--- | :--- |
| `Age` | Integer | `10` – `100` | Age of the student |
| `Gender` | String | `Male`, `Female` | Self-reported gender |
| `Country` | String | Any country name | Grouped into top countries or `Other` |
| `Academic_Level` | String | `High School`, `Undergraduate`, `Graduate` | Current level of study |
| `Most_Used_Platform` | String | `Instagram`, `YouTube`, `TikTok`, `Twitter`, etc. | Primary social media platform |
| `Purpose_Of_Use` | String | `Entertainment`, `Networking`, `Education`, `News` | Primary reason for social media use |
| `Avg_Daily_Usage_Hours` | Float | `0.0` – `24.0` | Total hours per day on social media |
| `Daily_Unlocks` | Integer | `0`+ | Approximate smartphone unlocks/day |
| `Study_Hours` | Float | `0.0` – `24.0` | Daily academic study hours |
| `Physical_Activity_Hours` | Float | `0.0` – `24.0` | Daily exercise or active hours |
| `Sleep_Hours_Per_Night` | Float | `0.0` – `24.0` | Average nightly sleep hours |
| `Stress_Level` | String | `Low`, `Medium`, `High`, `Very High` | Perceived daily stress baseline |

### Preprocessing & Modeling

- **Country Grouping**: Automatically preserves the top 10 represented countries (`India`, `USA`, `Canada`, `Australia`, `UK`, `Germany`, `Mexico`, `Turkey`, `France`) and maps others to `Other`.
- **ColumnTransformer**:
  - `StandardScaler` applied to continuous numerical indicators (usage hours, unlocks, study, sleep, exercise).
  - `OneHotEncoder` applied to categorical demographics and platform attributes.
  - `OrdinalEncoder` applied to academic and stress levels.
- **Model**: Linear Regression and Random Forest Regressors evaluated on $R^2$ score, Mean Absolute Error (MAE), and Mean Squared Error (MSE).

### Score Interpretation

The predicted mental health score ranges between **0.00** (highest strain) and **10.00** (strong resilience):

| Score Range | Signal Band | Interpretation |
| :--- | :--- | :--- |
| **0.00 – 3.99** | **Signal: Strained** | Elevated strain detected; habits may benefit from increased sleep or reduced screen time. |
| **4.00 – 6.99** | **Signal: Balanced** | Steady baseline with normal fluctuations; manageable stress-to-recovery ratio. |
| **7.00 – 10.00** | **Signal: Strong** | Resilient baseline supported by healthy sleep, physical activity, and moderate digital habits. |

---

## API Reference

### Endpoints

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/` | Serves the interactive frontend web dashboard (`index.html`) |
| `GET` | `/health` | Health-check endpoint verifying API status |
| `POST` | `/predict` | Evaluates student data and returns the predicted mental health score |
| `GET` | `/docs` | Interactive Swagger UI for live API testing |
| `GET` | `/redoc` | ReDoc API documentation |

### Example Request & Response

#### Request (`POST /predict`):
```bash
curl -X POST "http://127.0.0.1:8000/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "age": 21,
    "gender": "Female",
    "country": "India",
    "academic_level": "Undergraduate",
    "most_used_platform": "Instagram",
    "purpose_of_use": "Entertainment",
    "avg_daily_usage_hours": 4.5,
    "daily_unlocks": 85,
    "study_hours": 5.0,
    "physical_activity_hours": 1.0,
    "sleep_hours_per_night": 7.0,
    "stress_level": "Medium"
  }'
```

#### Response:
```json
{
  "predicted_mental_health_score": 6.82
}
```

---

## Local Development Setup

### 1. Prerequisites
- **Python 3.10+**
- **Git**

### 2. Clone the Repository
```bash
git clone https://github.com/sat-sde/Mental-Health-Score-main.git
cd Mental-Health-Score-main
```

### 3. Create a Virtual Environment
```bash
# macOS/Linux
python3 -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
venv\Scripts\activate
```

### 4. Install Dependencies
```bash
pip install -r requirements.txt
```

### 5. Run the Server
```bash
uvicorn main:app --reload --port 8000
```

Open your browser at `http://127.0.0.1:8000` to interact with the application.

---

## Deployment on Render

This project is optimized for deployment on [Render](https://render.com).

### Option A: Using Render Blueprint (Automatic)
1. Go to [Render Dashboard](https://dashboard.render.com).
2. Click **New +** > **Blueprint**.
3. Select your repository: **`sat-sde/Mental-Health-Score-main`**.
4. Render reads `render.yaml` automatically and configures the build and start commands.
5. Click **Apply**.

### Option B: Manual Web Service
1. Click **New +** > **Web Service**.
2. Connect `sat-sde/Mental-Health-Score-main`.
3. Set the following settings:
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
   - **Plan**: `Free`
4. Click **Create Web Service**.

Once deployed, your application will be live at `https://<your-service-name>.onrender.com`.

---

## Technology Stack

- **Machine Learning**: Scikit-Learn, Pandas, NumPy, Joblib
- **Backend API**: FastAPI, Uvicorn, Pydantic (v2)
- **Frontend**: Vanilla HTML5, Modern CSS3 (Grid, Flexbox, CSS Variables), ES6+ JavaScript
- **DevOps & Cloud**: Render, Git, GitHub

---

## Disclaimer

> **Important**: This application is built strictly for educational, research, and informational wellness tracking purposes. It utilizes statistical models to reflect data patterns and does **not** provide clinical diagnosis, psychiatric assessment, or medical counseling. If you or someone you know is experiencing acute stress, anxiety, or depression, please reach out to a certified healthcare professional or student counselor.
