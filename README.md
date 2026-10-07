# 🚀 Task Manager API

A production-ready CRUD REST API for managing personal tasks, built with Django REST Framework and JWT authentication. Features per-user data scoping, full input validation, and a live PostgreSQL deployment on Render.

## 🌐 Live Deployment
- **API base URL:** https://task-manager-api-11x0.onrender.com/api/
- **Frontend consuming this API:** [task-frontend](https://github.com/dakshkumawat07/task-frontend) — live at https://task-frontend-one-beta.vercel.app

> ⚠️ Hosted on Render's free tier, which spins down after 15 minutes of inactivity. The first request after idle may take 30–60 seconds to respond while the service wakes up.

## 📖 Project Overview
Built as an internship project to demonstrate a fully authenticated CRUD API from the ground up: user registration/login, JWT-protected routes, per-user task scoping, proper HTTP status codes, and a real cloud deployment with a managed PostgreSQL database — not just a local demo.

## ✨ Features
- User registration and JWT-based login (access + refresh tokens)
- Full CRUD on tasks (Create, Read, Update, Delete)
- Tasks are scoped per-user — you only ever see and modify your own
- Input validation with clear, structured error messages
- Proper HTTP status codes throughout (200/201/400/401/404)
- CORS configured to safely allow a separate frontend origin
- Environment-based configuration — no secrets committed to source control
- Static files served via WhiteNoise; production-served with Gunicorn

## 🛠 Tech Stack
| Layer | Technology |
|---|---|
| Language | Python 3 |
| Framework | Django + Django REST Framework |
| Auth | djangorestframework-simplejwt (JWT) |
| Database | PostgreSQL (production, Render) / SQLite (local dev) |
| Server | Gunicorn |
| Static files | WhiteNoise |
| Hosting | Render |
| Config | python-decouple + dj-database-url (environment variables) |

## ⚙️ Local Setup

```bash
git clone https://github.com/dakshkumawat07/task-manager-api.git
cd task-manager-api
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Create a `.env` file in the project root:
```env
SECRET_KEY=django-insecure-local-dev-key-change-in-prod
DEBUG=True
ALLOWED_HOSTS=127.0.0.1,localhost
DATABASE_URL=sqlite:///db.sqlite3
CORS_ALLOWED_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
```

Run migrations and start the server:
```bash
python manage.py migrate
python manage.py runserver
```

The API will be available at `http://127.0.0.1:8000/api/`.

## 🚢 Deployment

Deployed on [Render](https://render.com) as a Web Service, backed by a managed Render PostgreSQL instance.

- **Build Command:** `pip install -r requirements.txt && python manage.py collectstatic --noinput && python manage.py migrate`
- **Start Command:** `gunicorn core.wsgi --bind 0.0.0.0:$PORT`
- **Environment variables** (set via the Render dashboard, never committed): `SECRET_KEY`, `DEBUG`, `ALLOWED_HOSTS`, `DATABASE_URL`, `CORS_ALLOWED_ORIGINS`

> Render's free tier has no `release` command support, so database migrations run as part of the build step rather than as a separate release phase.

## 📡 API Endpoints

| Method | Endpoint | Auth required | Description |
|---|---|---|---|
| POST | `/api/auth/register/` | No | Register a new user. Body: `{"username", "password", "email"}` |
| POST | `/api/auth/login/` | No | Log in. Body: `{"username", "password"}` → returns `access` + `refresh` tokens |
| POST | `/api/auth/refresh/` | No | Refresh an access token. Body: `{"refresh"}` |
| GET | `/api/tasks/` | Yes | List your tasks |
| POST | `/api/tasks/` | Yes | Create a task. Body: `{"title", "description"}` |
| GET | `/api/tasks/{id}/` | Yes | Retrieve one task |
| PUT | `/api/tasks/{id}/` | Yes | Update a task |
| DELETE | `/api/tasks/{id}/` | Yes | Delete a task |

Protected endpoints require the header: `Authorization: Bearer <access_token>`

### Example request/response
```bash
curl -X POST https://task-manager-api-11x0.onrender.com/api/tasks/ \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  -d '{"title":"Finish internship task","description":"WA-4"}'
```
Response (`201`):
```json
{
  "id": 1,
  "title": "Finish internship task",
  "description": "WA-4",
  "completed": false,
  "created_at": "2026-10-06T16:36:39.763463Z"
}
```

## 🧠 Concepts Demonstrated
- DRF ViewSets and routers for CRUD scaffolding
- JWT authentication flow (access/refresh tokens)
- Scoping querysets to the requesting user for authorization (`get_queryset` override)
- Serializer-level input validation
- Environment-based settings for safe, portable deployment (12-factor config)
- Deploying a Django app with Gunicorn + WhiteNoise + a managed PostgreSQL database

## 🔭 Planned Improvements
- Pagination and filtering on the task list
- Task categories/tags
- Rate limiting on auth endpoints

## 👤 Author
Daksh Kumawat — [@dakshkumawat07](https://github.com/dakshkumawat07)
