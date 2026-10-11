# Frontend — To-Do App

Frontend for the [Backend Code](https://github.com/osama784/todo-app).

## What is the purpose of the project?

Allow the user to create, update, delete their daily tasks and mark them as completed — with categories to organize them.

## Deployment

No live deployment — run it locally (see below).

## Stack

Plain **HTML, CSS and JavaScript** — no framework, no build step.

- Font Awesome is vendored locally (`static/all.min.css` + `webfonts/`)
- The Cairo font is loaded from Google Fonts
- `fetch` calls the backend with token authentication; the token is stored in `localStorage`

> [!NOTE] <br/>
>
> You don't have to install any third-party packages.

## Features

- **Sign up / log in** (`signUp.html`, `login.html`) with client-side validation matching the backend rules (required fields, `@gmail.com` email, password of at least 8 characters)
- **Dashboard** (`index.html`) listing the user's tasks with filters: All Tasks / Done Tasks / New Tasks
- **Task actions:** create, edit, delete, and mark tasks as done or not done
- **Categories:** create, rename, delete, and assign categories to tasks; each category shows its task count, and the sidebar can filter tasks by category

## Files

- `index.html` — main dashboard
- `login.html`, `signUp.html` — authentication pages
- `static/js/api.js` — tasks and categories API calls
- `static/js/api_login.js`, `static/js/api_sign.js` — auth API calls
- `static/js/main.js`, `login.js`, `sign.js` — UI logic and form validation
- `static/styles/`, `static/images/` — styles and assets

## Run locally

Serve the folder with any static server and open `index.html`, for example:

```bash
python -m http.server 5500
```

Then visit `http://127.0.0.1:5500/index.html` (VS Code's Live Server on port 5500 also works). The pages call the API URL hardcoded in `static/js/*.js` (defaults to `http://127.0.0.1:8000`); change those URLs to point at your own backend instance if needed.

### Hope you love it ❤️

<hr/>

You could visit [My Website](https://osamadoage.vercel.app) 👌
