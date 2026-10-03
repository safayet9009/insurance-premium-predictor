Insurance Premium Predictor

A FastAPI practice project built to learn and implement the core concepts of FastAPI while integrating a Machine Learning model with a Streamlit frontend.

The project demonstrates how to build a REST API, validate incoming data using Pydantic, create computed fields, handle API requests, serve an ML model, and connect a backend API with a frontend application.

🎯 Project Purpose

This project was built primarily as a FastAPI learning and practice project.

The main goal is to understand how FastAPI can be used to build a backend API for a Machine Learning application.

Through this project, the following concepts are practiced:

FastAPI application setup
REST API development
HTTP methods
API endpoints and routing
Pydantic models
Request validation
Field() validation
Annotated
Literal
Computed fields
Path and query parameters
JSON responses
Exception handling
ML model serving
Frontend-backend communication
API documentation with Swagger UI
🏗️ Architecture
                 ┌──────────────────────┐
                 │        User          │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │     Streamlit UI     │
                 │       app.py         │
                 └──────────┬───────────┘
                            │
                       HTTP POST
                            │
                            ▼
                 ┌──────────────────────┐
                 │       FastAPI        │
                 │       main.py        │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Pydantic Model     │
                 │   Input Validation   │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │  Feature Engineering │
                 │                      │
                 │  BMI                 │
                 │  Age Group           │
                 │  Lifestyle Risk      │
                 │  City Tier           │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │    ML Model          │
                 │     model.pkl        │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   JSON Response      │
                 └──────────────────────┘
📂 Project Structure
insurance-premium-predictor/
│
├── main.py              # FastAPI backend
├── app.py               # Streamlit frontend
├── model.pkl            # Serialized ML model
├── requirements.txt     # Project dependencies
├── README.md            # Documentation
└── .gitignore           # Git ignore rules
🚀 FastAPI Concepts Practiced
1. FastAPI Application

The project starts by creating a FastAPI application:

from fastapi import FastAPI

app = FastAPI()

This application acts as the backend server.

2. API Routes

The project uses FastAPI route decorators to create API endpoints.

Example:

@app.post("/predict")
def predict_premium(data: UserInput):
    ...

The /predict endpoint accepts a POST request and returns the predicted insurance premium category.

3. Pydantic Data Validation

The project uses Pydantic models to validate incoming API data.

Example:

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

This allows FastAPI to automatically validate incoming JSON requests.

4. Field Validation

Field() is used to define validation rules and metadata.

Example:

age: Annotated[
    int,
    Field(..., gt=0, lt=120)
]

This ensures that the age is within the expected range.

5. Annotated

The project uses Python's Annotated type together with Pydantic's Field().

Example:

age: Annotated[int, Field(..., gt=0, lt=120)]

This keeps the type information and validation metadata together.

6. Literal

Literal is used to restrict a field to predefined values.

Example:

occupation: Literal[
    "retired",
    "freelancer",
    "student",
    "government_job",
    "business_owner",
    "unemployed",
    "private_job"
]

Invalid occupation values are automatically rejected by FastAPI.

7. Computed Fields

The project uses Pydantic's computed_field to calculate values from the user's input.

BMI
@computed_field
@property
def bmi(self) -> float:
    return self.weight / (self.height ** 2)
Age Group

The user's age is converted into categories such as:

young
adult
middle_aged
senior
Lifestyle Risk

Lifestyle risk is calculated using smoking status and BMI:

low
medium
high
City Tier

Cities are categorized into:

Tier 1
Tier 2
Tier 3

These concepts demonstrate how backend logic can be integrated into Pydantic models.

🔌 API Endpoint
POST /predict

This endpoint receives user information and returns an insurance premium category.

Request
{
  "age": 30,
  "weight": 65,
  "height": 1.7,
  "income_lpa": 10,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}
Response
{
  "predicted_category": "..."
}
📖 Interactive API Documentation

One of the major advantages of FastAPI is automatic API documentation.

After starting the server, open:

Swagger UI
http://127.0.0.1:8000/docs

Swagger allows the API to be tested directly from the browser.

ReDoc
http://127.0.0.1:8000/redoc

FastAPI automatically generates the API documentation based on the defined routes and Pydantic models.

🖥️ Streamlit Frontend

A simple Streamlit frontend is included to interact with the FastAPI backend.

The frontend collects:

Age
Weight
Height
Income
Smoking status
City
Occupation

It then sends the information to:

POST /predict

The prediction returned by FastAPI is displayed in the Streamlit interface.

🔄 Request Flow
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
🛠️ Tech Stack
Backend
Python
FastAPI
Pydantic
Uvicorn
Machine Learning
Scikit-learn
Pandas
NumPy
Pickle
Frontend
Streamlit
Requests
Development Tools
Git
GitHub
Python Virtual Environment
⚙️ Installation

Clone the repository:

git clone https://github.com/safayet9009/insurance-premium-predictor.git
cd insurance-premium-predictor

Create a virtual environment:

python3 -m venv venv

Activate it:

source venv/bin/activate

Install dependencies:

pip install -r requirements.txt
▶️ Run the FastAPI Backend

Start the FastAPI development server:

fastapi dev main.py

The API will run at:

http://127.0.0.1:8000
▶️ Run the Streamlit Frontend

Open another terminal:

cd insurance-premium-predictor
source venv/bin/activate
streamlit run app.py

The frontend will run at:

http://localhost:8501
🧪 Testing the API

The API can be tested using:

Swagger UI
ReDoc
Browser
cURL
Postman
Streamlit frontend

Example using cURL:

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
📚 What I Learned

This project helped me practice the following FastAPI concepts:

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

The project also provided hands-on experience with connecting a Machine Learning model to a production-style API structure.

🔮 Possible Future Improvements
Add proper exception handling
Add API authentication
Add database integration
Add automated testing with Pytest
Add logging
Add Docker support
Add environment variables
Add API versioning
Add model confidence and probabilities
Deploy the FastAPI backend
Add CI/CD pipeline
👨‍💻 Author

Safayet Hossain

Computer Science & Engineering Student

GitHub: https://github.com/safayet9009

📄 License

This project is created for learning and educational purposes.