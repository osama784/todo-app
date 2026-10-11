# TO DO — Backend

The REST API backend for a simple to-do app, built with **Django** and **Django REST Framework**. Users register and log in with token authentication, then manage their own tasks and categories.

- **Frontend:** [osama784/Frontend_TO_DO](https://github.com/osama784/Frontend_TO_DO)
- **Live demo:** [to-do-six-coral.vercel.app](https://to-do-six-coral.vercel.app) (the frontend; the API also runs separately on PythonAnywhere)

## Stack

- **Django 5.0.2**, **Django REST Framework 3.15.1**, **django-cors-headers 4.3.1**
- **Token authentication** (`rest_framework.authtoken`), plus session auth for the browsable API
- **SQLite** for development (`db.sqlite3` is committed)
- Dependencies are pinned in `requirements.txt` (the file is UTF-16 encoded — re-save it as UTF-8 if `pip install -r` complains)

## API overview

All endpoints were verified against the code in `tasks/`, `users/` and `api/`.

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

## How to run

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt

python manage.py migrate
python manage.py runserver
```

The API is then available at `http://127.0.0.1:8000/`, with Django admin at `/admin/` (create a superuser with `python manage.py createsuperuser`).

Example request once you have a token:

```bash
curl -H "Authorization: Token <your-token>" http://127.0.0.1:8000/tasks/
```

## Current state (honest notes)

This is a learning/portfolio project:

- Development settings are committed (`DEBUG = True`, generated secret key) — not production-ready.
- The test files are Django placeholders; there is no test suite.
- The deployed frontend calls the API at `https://osamaTasks.pythonanywhere.com` (hardcoded in the frontend's JS).

---

Built by [Osama Doage](https://osamadoage.vercel.app).
