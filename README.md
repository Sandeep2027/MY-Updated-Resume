KPA Form API Project
Overview
This project implements a form submission system with a FastAPI backend and an HTML-based frontend. The backend handles user authentication, form data submission with file uploads, and paginated data retrieval, storing data in a PostgreSQL database. The frontend provides a login page and a form page for submitting and viewing form data.
Project Structure
kpa-form-api/
├── backend/
│   ├── main.py
│   ├── database.py
│   ├── requirements.txt
│   ├── .env
│   └── Dockerfile
├── frontend/
│   ├── login.html
│   ├── form.html
├── docker-compose.yml
├── KPA_form_data_updated.postman_collection.json
└── README.md

Setup Instructions
Prerequisites

Python 3.11: For running the backend and serving the frontend locally.
Verify: python --version
Download: python.org


Docker: Optional, for running the backend and PostgreSQL.
Download: docker.com


Git: For cloning the repository.
Verify: git --version


Web Browser: Chrome, Firefox, or Edge for accessing HTML pages.
Postman: Optional, for API testing.
Download: postman.com



Steps

Clone the Repository:
git clone <your-github-repo>
cd kpa-form-api


Backend Setup:

Create a .env file in backend/ with:DATABASE_URL=postgresql://postgres:password123@localhost:5432/kpa_db
API_BASE_URL=http://localhost:8000
JWT_SECRET=your_jwt_secret_key_1234567890
JWT_ALGORITHM=HS256


Option 1: Run with Docker:
Ensure Docker Desktop is running.
Run:docker-compose up --build


API available at: http://localhost:8000
Verify: Open http://localhost:8000/docs


Option 2: Run without Docker:
Install PostgreSQL: postgresql.org
Create database:psql -U postgres -c "CREATE DATABASE kpa_db;"


Install dependencies:cd backend
pip install -r requirements.txt


Run FastAPI:uvicorn main:app --host 0.0.0.0 --port 8000


Verify: Open http://localhost:8000/docs




Frontend Setup:

Navigate to frontend/:cd frontend


Serve HTML files locally (required for CORS):python -m http.server 8080


Access at: http://localhost:8080/login.html
Alternative: Use VS Code with Live Server:
Install VS Code: code.visualstudio.com
Open frontend/ in VS Code:code .


Install “Live Server” extension (ritwickdey.LiveServer).
Right-click login.html > “Open with Live Server” (e.g., http://localhost:5500/login.html).




Testing with Postman:

Import KPA_form_data_updated.postman_collection.json.
Set baseUrl to http://localhost:8000.
Test /api/login:
Body: {"phone": "7760873976", "password": "to_share@123"}
Save access_token to token variable.


Test /api/submitForm (POST) with form-data:
Headers: Authorization: Bearer {{token}}
Fields: name, phone, email, address, optional file (JPEG, PNG, PDF, <5MB)


Test /api/getFormData (GET) with pagination:
Headers: Authorization: Bearer {{token}}
Query: page=1, page_size=10





Technologies Used

Backend:
Python 3.11
FastAPI 0.115.0
PostgreSQL 15
SQLAlchemy 2.0.35
Uvicorn 0.30.0
Python-dotenv
PyJWT 2.9.0
python-jose[cryptography] 3.3.0
Docker


Frontend:
HTML5, CSS3, JavaScript (ES6)
Fetch API


Testing: Postman

Implemented APIs

POST /api/login

Authenticates users and returns a JWT token.
Request: JSON with phone and password.
Response: JSON with access_token, token_type.
Status Codes: 200 (success), 401 (invalid credentials), 500 (server error)


POST /api/submitForm

Submits a form-data with optional file upload to PostgreSQL.
Request: Form-data with name, phone (10 digits), email, address, optional file (JPEG, PNG, PDF, <5MB).
Headers: Authorization: Bearer <token>
Response: JSON object with submitted data and ID.
Validation: Phone (10 digits), email format, valid file type, file size (<5MB).
Status Codes: 200 (success), 400 (invalid input), 401 (unauthorized), 500 (server error)


GET /api/getFormData

Retrieves a paginated form-data from a PostgreSQL.
Request: Query parameters to page (default 1), page_size (default 10).
Headers: Authorization: Bearer <token>
Response: List of JSON form-data objects.
Status Codes: 200 (success), 401 (unauthenticated), 500 (server error)



Advanced Features

Backend:
JWT authentication for secure endpoints.
Pagination for efficient data retrieval.
File size validation (5MB limit).
Error logging to backend/api.log.
Database index on created_at for faster queries.


Frontend:
Login page for user authentication.
Form submission with validation and file upload.
Paginated table for viewing submitted forms.
Loading indicators, error/success messages.
Logout functionality.



Usage

Login:
Open http://localhost:8080/login.html.
Enter phone: 7760873976, password: to_share@123.
Redirects to form.html on success.


Form Submission:
Fill in name, phone (10 digits), email, address.
Upload optional file (JPEG, PNG, PDF, <5MB).
Submit to see success message and updated table.


View Data:
Table displays submitted forms with pagination.
Use Previous/Next buttons to navigate pages.


Logout:
Click “Logout” to clear token and return to login page.



Troubleshooting

Backend Issues:
Ensure Docker/PostgreSQL is running.
Check backend/api.log for errors.
Verify port 8000 is free: netstat -aon | findstr :8000


CORS Errors:
Serve frontend via python -m http.server or Live Server.


Login Failure:
Verify credentials and JWT_SECRET in .env.


Empty Table:
Submit forms first via frontend or Postman.
Check database: psql -h localhost -U postgres -d kpa_db -c "SELECT * FROM form_data;"



Limitations

Hardcoded credentials (main.py) for simplicity; use a database in production.
File uploads limited to 5MB and JPEG, PNG, or PDF.
Pagination defaults to 10 records per page.
Token expires after 24 hours.

Screen Recording

Link: Google Drive Link
Description: Shows backend setup, Postman API testing (login, submitForm, getFormData), and frontend usage (login, form submission, data retrieval).
