# 🏥 Insurance Premium Predictor

A **FastAPI practice project** built to learn and implement the core concepts of **FastAPI, Pydantic, REST APIs, and ML model serving**, with a **Streamlit frontend** for user interaction.

The project demonstrates how a Machine Learning model can be exposed through a REST API and consumed by a separate frontend application.

---

## 🎯 Project Purpose

This project was primarily built as a **FastAPI learning and practice project**.

The main objective was to understand how FastAPI can be used to build a backend API for a Machine Learning application.

### Concepts Practiced

* FastAPI application setup
* REST API development
* HTTP methods
* API routing
* Pydantic models
* Request validation
* `Field()` validation
* `Annotated`
* `Literal`
* Computed fields
* Feature engineering
* JSON responses
* Exception handling
* ML model serving
* Frontend–backend communication
* Automatic API documentation
* Swagger UI
* ReDoc

---

## 🏗️ Architecture

```text
                         ┌──────────────────┐
                         │       User       │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │   Streamlit UI   │
                         │      app.py      │
                         └────────┬─────────┘
                                  │
                             HTTP POST
                                  │
                                  ▼
                         ┌──────────────────┐
                         │     FastAPI      │
                         │      main.py     │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │ Pydantic Model   │
                         │ Input Validation │
                         └────────┬─────────┘
                                  │
                                  ▼
                       ┌──────────────────────┐
                       │ Feature Engineering │
                       │                      │
                       │ • BMI                │
                       │ • Age Group          │
                       │ • Lifestyle Risk     │
                       │ • City Tier          │
                       └──────────┬───────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │    ML Model      │
                         │    model.pkl     │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │  JSON Response   │
                         └──────────────────┘
```

---

## 📂 Project Structure

```text
insurance-premium-predictor/
│
├── main.py              # FastAPI backend
├── app.py               # Streamlit frontend
├── model.pkl            # Serialized ML model
├── requirements.txt     # Python dependencies
├── README.md            # Project documentation
└── .gitignore           # Git ignore rules
```

---

# 🚀 FastAPI Concepts Practiced

## 1. FastAPI Application

The project starts by creating a FastAPI application:

```python
from fastapi import FastAPI

app = FastAPI()
```

This `app` object acts as the main backend application.

---

## 2. API Routes

FastAPI route decorators are used to create API endpoints.

Example:

```python
@app.post("/predict")
def predict_premium(data: UserInput):
    ...
```

The `/predict` endpoint accepts a `POST` request and returns the predicted insurance premium category.

---

## 3. Pydantic Models

Pydantic models are used to define and validate incoming request data.

Example:

```python
class UserInput(BaseModel):
    age: int
    weight: float
    height: float
    income_lpa: float
    smoker: bool
    city: str
    occupation: Literal[
        "retired",
        "freelancer",
        "student",
        "government_job",
        "business_owner",
        "unemployed",
        "private_job"
    ]
```

FastAPI automatically uses this model to validate incoming JSON data.

---

## 4. Field Validation

`Field()` is used to define validation rules and additional metadata.

Example:

```python
age: Annotated[
    int,
    Field(..., gt=0, lt=120)
]
```

This ensures that the provided age is greater than `0` and less than `120`.

---

## 5. Annotated

The project uses Python's `Annotated` type together with Pydantic's `Field()`.

Example:

```python
age: Annotated[
    int,
    Field(..., gt=0, lt=120)
]
```

This allows type information and validation rules to be defined together.

---

## 6. Literal

`Literal` restricts a field to a predefined set of values.

Example:

```python
occupation: Literal[
    "retired",
    "freelancer",
    "student",
    "government_job",
    "business_owner",
    "unemployed",
    "private_job"
]
```

If an invalid occupation is provided, FastAPI automatically returns a validation error.

---

## 7. Computed Fields

The project uses Pydantic's `computed_field` to derive additional information from the user's input.

### BMI

```python
@computed_field
@property
def bmi(self) -> float:
    return self.weight / (self.height ** 2)
```

BMI is calculated from weight and height.

### Age Group

The user's age is converted into one of the following groups:

| Age     | Group       |
| ------- | ----------- |
| `< 25`  | Young       |
| `25–44` | Adult       |
| `45–59` | Middle-aged |
| `60+`   | Senior      |

### Lifestyle Risk

Lifestyle risk is derived from smoking status and BMI:

```text
Low
Medium
High
```

### City Tier

Cities are categorized into:

```text
Tier 1
Tier 2
Tier 3
```

These computed features are then passed to the ML pipeline for prediction.

---

# 🔌 API Endpoint

## `POST /predict`

The main API endpoint receives user information and returns an insurance premium category.

### Request

```json
{
  "age": 30,
  "weight": 65,
  "height": 1.7,
  "income_lpa": 10,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}
```

### Response

```json
{
  "predicted_category": "..."
}
```

---

# 📖 Interactive API Documentation

One of FastAPI's major advantages is its automatic API documentation.

After starting the backend, the following interfaces are available.

### Swagger UI

```text
http://127.0.0.1:8000/docs
```

Swagger UI allows API endpoints to be tested directly from the browser.

### ReDoc

```text
http://127.0.0.1:8000/redoc
```

FastAPI automatically generates these documentation interfaces from the API routes and Pydantic models.

---

# 🖥️ Streamlit Frontend

A simple Streamlit frontend is included to interact with the FastAPI backend.

The frontend collects:

* Age
* Weight
* Height
* Annual Income
* Smoking Status
* City
* Occupation

The collected information is sent to:

```text
POST /predict
```

The prediction returned by FastAPI is then displayed in the Streamlit interface.

---

# 🔄 Request Flow

```text
User
 │
 ▼
Streamlit Form
 │
 ▼
HTTP POST Request
 │
 ▼
FastAPI /predict
 │
 ▼
Pydantic Validation
 │
 ▼
Computed Fields
 │
 ├── BMI
 ├── Age Group
 ├── Lifestyle Risk
 └── City Tier
 │
 ▼
ML Model
 │
 ▼
Prediction
 │
 ▼
JSON Response
 │
 ▼
Streamlit UI
```

---

# 🛠️ Tech Stack

| Category        | Technologies  |
| --------------- | ------------- |
| Language        | Python        |
| Backend         | FastAPI       |
| Validation      | Pydantic      |
| Server          | Uvicorn       |
| ML              | Scikit-learn  |
| Data Processing | Pandas, NumPy |
| Frontend        | Streamlit     |
| HTTP Client     | Requests      |
| Serialization   | Pickle        |
| Version Control | Git, GitHub   |

---

# ⚙️ Installation

## 1. Clone the Repository

```bash
git clone https://github.com/safayet9009/insurance-premium-predictor.git
cd insurance-premium-predictor
```

## 2. Create a Virtual Environment

```bash
python3 -m venv venv
```

## 3. Activate the Virtual Environment

### Linux / macOS

```bash
source venv/bin/activate
```

### Windows

```bash
venv\Scripts\activate
```

## 4. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# ▶️ Running the Application

The application consists of two components:

1. FastAPI backend
2. Streamlit frontend

Both need to be running simultaneously.

---

## Start FastAPI Backend

```bash
fastapi dev main.py
```

The API will be available at:

```text
http://127.0.0.1:8000
```

---

## Start Streamlit Frontend

Open another terminal:

```bash
cd insurance-premium-predictor
source venv/bin/activate
streamlit run app.py
```

The Streamlit application will be available at:

```text
http://localhost:8501
```

---

# 🧪 Testing the API

The API can be tested using:

* Swagger UI
* ReDoc
* cURL
* Postman
* Streamlit frontend

### cURL Example

```bash
curl -X POST http://127.0.0.1:8000/predict \
-H "Content-Type: application/json" \
-d '{
  "age": 30,
  "weight": 65,
  "height": 1.7,
  "income_lpa": 10,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}'
```

---

# 📚 What I Learned

This project provided hands-on practice with the following FastAPI concepts:

```text
FastAPI Application
        ↓
Routing
        ↓
HTTP Methods
        ↓
Pydantic Models
        ↓
Data Validation
        ↓
Annotated
        ↓
Field()
        ↓
Literal
        ↓
Computed Fields
        ↓
Request Handling
        ↓
JSON Responses
        ↓
ML Model Serving
        ↓
Frontend ↔ Backend Communication
        ↓
Automatic API Documentation
```

The project also helped me understand how a Machine Learning model can be integrated into a backend API and consumed by a separate frontend application.

---

# 🔮 Future Improvements

Possible improvements include:

* [ ] Add more API endpoints
* [ ] Improve exception handling
* [ ] Add automated tests with Pytest
* [ ] Add API authentication
* [ ] Add database integration
* [ ] Add structured logging
* [ ] Add environment variables
* [ ] Add Docker support
* [ ] Add API versioning
* [ ] Add model confidence and class probabilities
* [ ] Deploy the FastAPI backend
* [ ] Add CI/CD pipeline

---

# 👨‍💻 Author

**Safayet Hossain**

Computer Science & Engineering Student

GitHub: [@safayet9009](https://github.com/safayet9009)

---

## 📄 License

This project was created for **learning and educational purposes**.
