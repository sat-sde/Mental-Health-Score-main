# Mental Health Score (Mansik Santulan Score)

A machine learning web application to analyze and predict a student's mental health score based on social media usage, academic level, physical activity, sleep habits, and stress levels.

## Features

- **FastAPI Backend**: High-performance REST API predicting mental health scores using a trained scikit-learn regression model (`Mental_Health_Model.pkl`).
- **Interactive Web UI**: Modern interface built with HTML, CSS, and JavaScript allowing users to input metrics and view real-time score evaluations.
- **Machine Learning Analysis**: Jupyter Notebook (`ML_Project.ipynb`) detailing dataset analysis, feature engineering, and model training.

## Tech Stack

- **Backend**: Python, FastAPI, Uvicorn, Pydantic, Scikit-learn, Pandas, Joblib
- **Frontend**: HTML5, CSS3, JavaScript (Fetch API)
- **Data & Modeling**: Jupyter Notebook, Matplotlib/Seaborn, NumPy, Pandas

## Getting Started

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Run the Backend API
```bash
uvicorn main:app --reload
```
The API server will run at `http://127.0.0.1:8000`. You can test endpoints via Swagger docs at `http://127.0.0.1:8000/docs`.

### 3. Launch Frontend
Open `index.html` in your browser or serve it with a local static web server.
