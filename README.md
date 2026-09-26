# 🚀 Task Manager API

A CRUD REST API for managing personal tasks, built with Django REST Framework and JWT authentication.

## 📖 Project Overview
Built as an internship task (WA-2) to demonstrate a full authenticated CRUD API — user registration/login, per-user task scoping, and proper HTTP status codes throughout.

## ✨ Features
- User registration and JWT-based login
- Full CRUD on tasks (Create, Read, Update, Delete)
- Tasks are scoped per-user — you only ever see your own
- Input validation with clear error messages
- Proper HTTP status codes (200/201/400/401/404)

## 🛠 Tech Stack
- Python 3, Django, Django REST Framework
- djangorestframework-simplejwt (JWT auth)
- SQLite (development database)

## ⚙️ How to Use

### Setup
\`\`\`bash
git clone https://github.com/dakshkumawat07/task-manager-api.git
cd task-manager-api
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
\`\`\`

### API Endpoints

| Method | Endpoint | Auth required | Description |
|---|---|---|---|
| POST | /api/auth/register/ | No | Register a new user. Body: `{"username", "password", "email"}` |
| POST | /api/auth/login/ | No | Log in. Body: `{"username", "password"}` → returns `access` + `refresh` tokens |
| POST | /api/auth/refresh/ | No | Refresh an access token. Body: `{"refresh"}` |
| GET | /api/tasks/ | Yes | List your tasks |
| POST | /api/tasks/ | Yes | Create a task. Body: `{"title", "description"}` |
| GET | /api/tasks/{id}/ | Yes | Retrieve one task |
| PUT | /api/tasks/{id}/ | Yes | Update a task |
| DELETE | /api/tasks/{id}/ | Yes | Delete a task |

Protected endpoints require header: `Authorization: Bearer <access_token>`

### Example request/response
\`\`\`bash
curl -X POST http://127.0.0.1:8000/api/tasks/ \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer <TOKEN>" \\
  -d '{"title":"Finish internship task","description":"WA-2"}'
\`\`\`
Response (201):
\`\`\`json
{"id": 1, "title": "Finish internship task", "description": "WA-2", "completed": false, "created_at": "2026-09-26T06:05:45.070717Z"}
\`\`\`

## 🧠 Concepts Learned
- DRF ViewSets and routers for CRUD scaffolding
- JWT authentication flow (access/refresh tokens)
- Scoping querysets to the requesting user for authorization
- Serializer-level input validation

## 🔭 Planned Improvements
- Pagination and filtering on the task list
- Task categories/tags
- Rate limiting on auth endpoints

## 👤 Author
Daksh Kumawat — [@dakshkumawat07](https://github.com/dakshkumawat07)
