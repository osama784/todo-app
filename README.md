# To-Do App

A full-stack to-do app: a **Django REST API** backend and a **vanilla JavaScript** frontend.

- **Live demo (frontend):** [to-do-six-coral.vercel.app](https://to-do-six-coral.vercel.app)
- **API:** [osamaTasks.pythonanywhere.com](https://osamaTasks.pythonanywhere.com) (the backend, deployed separately on PythonAnywhere)

## Structure

| Folder | What's inside |
|--------|---------------|
| `backend/` | Django 5.0.2 + Django REST Framework — token auth, tasks and categories |
| `frontend/` | Plain HTML, CSS and JavaScript — no framework, no build step |

## Stack

**Backend**

- **Django 5.0.2**, **Django REST Framework 3.15.1**, **django-cors-headers 4.3.1**
- **Token authentication** (`rest_framework.authtoken`), plus session auth for the browsable API
- **SQLite** for development (`backend/db.sqlite3` is committed)
- Dependencies are pinned in `backend/requirements.txt` (the file is UTF-16 encoded — re-save it as UTF-8 if `pip install -r` complains)

**Frontend**

- Plain **HTML, CSS and JavaScript** — no framework, no build step
- Font Awesome is vendored locally (`frontend/static/all.min.css` + `frontend/webfonts/`); the Cairo font is loaded from Google Fonts
- `fetch` calls the backend with token authentication; the token is stored in `localStorage`

## Quick start

### Backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt

python manage.py migrate
python manage.py runserver
```

The API is then available at `http://127.0.0.1:8000/`, with Django admin at `/admin/` (create a superuser with `python manage.py createsuperuser`).

> [!NOTE]
> `backend/requirements.txt` is UTF-16 encoded — re-save it as UTF-8 if `pip install -r` complains.

Example request once you have a token:

```bash
curl -H "Authorization: Token <your-token>" http://127.0.0.1:8000/tasks/
```

### Frontend

Serve `frontend/` with any static server and open `index.html`, for example:

```bash
cd frontend
python -m http.server 5500
```

Then visit `http://127.0.0.1:5500/index.html` (VS Code's Live Server on port 5500 also works). The pages call the deployed API URL hardcoded in `frontend/static/js/*.js`; change those URLs to point at your own backend instance if needed.

## Features

- **Sign up / log in** (`frontend/signUp.html`, `frontend/login.html`) with client-side validation matching the backend rules (required fields, `@gmail.com` email, password of at least 8 characters); registration returns an auth token
- **Tasks CRUD:** create, edit, delete, and mark tasks as done or not done
- **Categories:** create, rename, delete, and assign categories to tasks; each category shows its task count, and the sidebar can filter tasks by category
- **Filters:** All Tasks / Done Tasks / New Tasks
- **Token authentication:** all task/category endpoints require auth and are scoped to the logged-in user

## API overview

All endpoints were verified against the code in `backend/tasks/`, `backend/users/` and `backend/api/`.

### Auth and users — `/users/`

| Method | Endpoint | What it does |
|--------|----------|--------------|
| POST | `/users/create/` | Register with `username`, `email`, `password`; returns an auth token |
| POST | `/users/login/` | Log in; returns an auth token |
| GET | `/users/` | Get the current user (requires token) |

Registration validates: unique username, an email ending in `@gmail.com`, and Django's standard password validators.

### Tasks — `/tasks/`

| Method | Endpoint | What it does |
|--------|----------|--------------|
| GET | `/tasks/` | List the current user's tasks. Query params: `q_category=<id>`, `q_done=True\|False` |
| POST | `/tasks/create/` | Create a task (`name`, `body`, optional `category`) |
| PUT / PATCH | `/tasks/<pk>/update/` | Update a task (owner only) |
| DELETE | `/tasks/<pk>/delete/` | Delete a task (owner only) |

A task has a `name`, `body`, optional `category`, `is_done` flag, and creation date. Marking a task done is a `PATCH` to the update endpoint with `is_done`.

### Categories — `/tasks/categories/`

| Method | Endpoint | What it does |
|--------|----------|--------------|
| GET | `/tasks/categories/` | List the user's categories, each with its task count, plus the user's total task count |
| POST | `/tasks/categories/create/` | Create a category |
| PUT / PATCH | `/tasks/categories/<pk>/update/` | Rename a category (owner only) |
| DELETE | `/tasks/categories/<pk>/delete/` | Delete a category (owner only; tasks keep existing, category is set to null) |

There is also `GET /api/` returning a small route index. All task/category endpoints require authentication and are scoped to the logged-in user.

## Current state (honest notes)

This is a learning/portfolio project:

- Development settings are committed (`DEBUG = True`, generated secret key) — not production-ready.
- `backend/requirements.txt` is UTF-16 encoded — re-save it as UTF-8 if `pip install -r` complains.
- The test files are Django placeholders; there is no test suite.
- The deployed frontend calls the API at `https://osamaTasks.pythonanywhere.com` (hardcoded in `frontend/static/js/*.js`).

---

Built by [Osama Doage](https://osamadoage.vercel.app).
