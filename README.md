# 🩺 Health Risk API

A simple Flask-based REST API that estimates health risk using basic blood test values (glucose, haemoglobin, cholesterol) and returns:

- A risk level: **Low**, **Moderate**, or **High**
- A human-readable **remark** explaining which values are abnormal

This API is used by the **MediRisk 360** Streamlit health prediction application as its external AI/ML health service.

---

## 🌐 Live Endpoint (Render)



**Base URL:**

https://health-risk-api-0lsl.onrender.com

**Prediction endpoint (POST):**

`/predict`

---

## 📮 Request Format

### URL

```text
POST /predict
Content-Type: application/json
```

### JSON body

```json
{
  "glucose": 120,
  "haemoglobin": 14,
  "cholesterol": 180
}
```

All three values must be numeric.

---

## 📤 Response Format

On success (`200 OK`):

```json
{
  "risk_level": "Low",
  "remark": "Low risk: All lab values are within approximate normal ranges."
}
```

On invalid/missing input (`400 Bad Request`):

```json
{
  "error": "Invalid or missing input values"
}
```

---

## 🧠 Risk Logic (Rule-based model)

The API uses simple rule-based logic over approximate reference ranges:

- Glucose: 70–140  
- Haemoglobin: 12–16  
- Cholesterol: 125–200  

- If all values are within range → **Low** risk.  
- If exactly one value is abnormal → **Moderate** risk with a remark naming that value.  
- If two or more values are abnormal → **High** risk with a remark listing all abnormal values.

> This is for demonstration and learning purposes only, not real medical advice.

---

## 🧪 Running the API Locally

1. **Clone the repository**

   ```bash
   git clone https://github.com/<your-username>/health-risk-api.git
   cd health-risk-api
   ```

2. **(Optional) Create and activate a virtual environment**

   ```bash
   python -m venv venv
   # Windows
   venv\Scripts\activate
   # macOS / Linux
   source venv/bin/activate
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Run the Flask app (development mode)**

   ```bash
   python risk_api.py
   ```

   The API will be available at `http://0.0.0.0:8000/predict`.

---

## 🚀 Deployment on Render

This repository is configured to run on **Render** as a Web Service.

- **Build Command**

  ```bash
  pip install -r requirements.txt
  ```

- **Start Command**

  ```bash
  gunicorn risk_api:app
  ```

Render runs `gunicorn` with the Flask application object `app` defined in `risk_api.py`.

---

## 📁 Project Structure

```text
.
├── risk_api.py       # Main Flask application exposing /predict
├── requirements.txt  # Python dependencies (Flask, Flask-CORS, gunicorn)
└── README.md         # API documentation
```

---

## ⚠️ Disclaimer

- This API is **for educational/demo purposes only** and is **not** a medical device.  
- Do not use the responses for real diagnosis or treatment decisions; always consult a qualified healthcare professional.
