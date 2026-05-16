# 🏥 Insurance Premium Category Predictor

A machine learning-powered REST API that predicts a user's **insurance premium category** based on personal and demographic information. Built with **FastAPI** for the backend and **Streamlit** for an interactive frontend UI.

---

## 📋 Overview

This project serves a pre-trained ML model via a FastAPI endpoint. Users can submit their personal details — age, weight, height, income, smoking status, city, and occupation — and the model returns a predicted insurance premium category. A Streamlit frontend provides a clean browser-based interface to interact with the API without needing tools like Postman or curl.

---
<img width="827" height="384" alt="image" src="https://github.com/user-attachments/assets/208c2b90-31e3-432f-80b0-687f82c52f8f" />

## 🗂️ Project Structure

```
ML_Fast_API/
├── main.py        # FastAPI app with the /predict endpoint and input validation
├── frontend.py    # Streamlit UI that calls the FastAPI backend
└── model.pkl      # Pre-trained scikit-learn ML model
```

---

## ⚙️ How It Works

### Feature Engineering (done automatically in the API)

The API accepts raw user inputs and derives the following features before passing them to the model:

| Derived Feature   | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| `bmi`             | Calculated from weight and height (`weight / height²`)                      |
| `age_group`       | Bucketed into `young`, `adult`, `middle_aged`, or `senior`                 |
| `lifestyle_risk`  | `high`, `medium`, or `low` based on BMI and smoking status                 |
| `city_tier`       | `1`, `2`, or `3` based on the city's classification in India               |

### API Endpoint

**`POST /predict`**

**Request Body:**

```json
{
  "age": 35,
  "weight": 75.0,
  "height": 1.75,
  "income_lpa": 12.5,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}
```

**Response:**

```json
{
  "predicted_category": "<insurance premium tier>"
}
```

**Supported `occupation` values:** `retired`, `freelancer`, `student`, `government_job`, `business_owner`, `unemployed`, `private_job`

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- pip

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Shuvo018/ML_Fast_API.git
   cd ML_Fast_API
   ```

2. **Install dependencies:**
   ```bash
   pip install fastapi uvicorn pydantic pandas streamlit requests
   ```

### Running the App

**Step 1 — Start the FastAPI backend:**
```bash
uvicorn main:app --reload
```
The API will be available at `http://127.0.0.1:8000`.

**Step 2 — Launch the Streamlit frontend** (in a separate terminal):
```bash
streamlit run frontend.py
```
The UI will open in your browser at `http://localhost:8501`.

### Interactive API Docs

FastAPI automatically generates documentation. Once the backend is running, visit:
- **Swagger UI:** `http://127.0.0.1:8000/docs`
- **ReDoc:** `http://127.0.0.1:8000/redoc`

---

## 🛠️ Tech Stack

| Layer     | Technology                  |
|-----------|-----------------------------|
| API       | FastAPI                     |
| Validation| Pydantic v2                 |
| Frontend  | Streamlit                   |
| ML Model  | scikit-learn (via pickle)   |
| Data      | pandas                      |

---

## 📌 Notes

- The model (`model.pkl`) must be present in the root directory for the API to start.
- City classification covers major **Tier 1** and **Tier 2** Indian cities. Any unrecognized city is automatically treated as **Tier 3**.
- Input validation is enforced by Pydantic (e.g., age must be between 1–150, weight and height must be positive).

---

## 📄 License

This project is open source. Feel free to use, modify, and distribute it.
