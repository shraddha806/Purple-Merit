# Purple Merit

**Small Flask app with user authentication, task management, and a demo admin UI.**

---

## 🚀 Overview

Purple Merit is a lightweight Flask project that provides an API and a small frontend (served from `templates/` and `static/`) for user authentication and task management. It includes JWT-based auth, simple task CRUD with auto-generated summaries/tags, and a demo admin flow.


## 🧭 Project structure

- `backend/` — Flask application
  - `app.py` — application factory and routes registration
  - `manage.py` — shell context and run helper
  - `config.py` — configuration object
  - `extensions.py` — initialized Flask extensions (SQLAlchemy, Migrate, Bcrypt, JWT)
  - `models.py` — `User` and `Task` models
  - `routes/` — blueprints: `auth`, `tasks`, `user`, `admin`
  - `templates/`, `static/` — simple frontend pages and assets
  - `tests/` — pytest tests (example: `tests/test_auth.py`)


## ⚙️ Requirements & prerequisites

- Python 3.11+ (this repo uses a venv under `Task/`)
- pip
- (Optional) `virtualenv` or `venv`

Install dependencies:

```bash
cd backend
python -m pip install -r requirements.txt
```


## 🛠️ Setup (Windows / PowerShell)

1. Create & activate virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
pip install -r backend/requirements.txt
```

3. Configure environment variables (create a `.env` file or set in your shell):

```
# Example .env (DO NOT commit secrets)
FLASK_ENV=development
FLASK_APP=manage.py
SECRET_KEY=your-secret-here
DATABASE_URL=sqlite:///dev.db        # or your DB URI
JWT_SECRET_KEY=your-jwt-secret-here
```

> 🔧 Note: `FLASK_APP` should reference `manage.py` when running `flask` CLI commands from inside `backend/`.


## 🗄️ Database & migrations

This project uses Flask-Migrate (Alembic). From the `backend/` folder:

```powershell
# set FLASK_APP in PowerShell
$env:FLASK_APP = 'manage.py'
# initialize migrations (only needed once)
flask db init
flask db migrate -m "Initial"
flask db upgrade
```

Alternatively, you can let the development server auto-create tables in debug mode (this is done in `create_app` when `app.debug` is `True`).


## ▶️ Running the app

From the `backend/` directory:

```powershell
python manage.py        # start dev server (uses create_app())
# or
$env:FLASK_APP='manage.py'
flask run
```

The app listens on `http://127.0.0.1:5000/` by default.


## 🧪 Running tests

From repository root or `backend/` folder:

```bash
pip install -r backend/requirements.txt
cd backend
pytest -q
```


## 🔌 API Highlights

- `GET /` — app index (returns frontend HTML if `Accept: text/html`)
- `GET /health` — health check
- `POST /api/auth/signup` — register (JSON: `email`, `password`, `full_name`)
- `POST /api/auth/login` — login (returns `access_token` JWT)
- `POST /api/auth/logout` — logout (requires `Authorization: Bearer <token>`)
- `GET /api/auth/me` — current user (JWT)
- `GET /seed-admin` — create a demo admin user (development only)

Tasks (JWT required):
- `GET /api/tasks` — list
- `POST /api/tasks` — create (title required; summary/tags may be auto-generated)
- `GET /api/tasks/<id>` — retrieve
- `PUT /api/tasks/<id>` — update
- `DELETE /api/tasks/<id>` — delete
- `POST /api/tasks/ai-simulate` — summary/tags simulation

Example: create an account and create a task:

```bash
# signup
curl -X POST http://127.0.0.1:5000/api/auth/signup -H "Content-Type: application/json" -d '{"email":"me@example.com","password":"Password1!","full_name":"Me"}'
# login
curl -X POST http://127.0.0.1:5000/api/auth/login -H "Content-Type: application/json" -d '{"email":"me@example.com","password":"Password1!"}'
# use returned access_token in Authorization header: Authorization: Bearer <token>
```


## ⚠️ Security & notes

- The JWT token blacklist used for logout is an in-memory `set` (in `extensions.py`) — this is suitable for demos/tests only. Use persistent revocation storage for production.
- The app auto-creates tables in debug mode; for production, use proper migrations and DB backups.


## 🤝 Contributing

Contributions are welcome. Open an issue or a PR describing changes. Keep changes small and test-covered where feasible.


## 📄 License

Add a `LICENSE` file to document the project license (e.g., MIT) if desired.


---

If you'd like, I can add: CI steps, a CONTRIBUTING guide, example Postman collection, or expand the API reference with example request/response bodies. ✅
