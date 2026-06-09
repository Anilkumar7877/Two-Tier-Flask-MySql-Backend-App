 
# two-tier-flask-mysql-backend-app

Lightweight Flask app backed by MySQL and runnable with Docker Compose.

What this repo contains
- A Flask backend (`app.py`) that stores and displays messages.
- A `Dockerfile` to build the Flask image.
- A `docker-compose.yml` that runs MySQL and the Flask service together.
- A seed file: `message.sql` (optional — the app creates the table on startup).

Prerequisites
- Docker and Docker Compose (or the `docker` and `docker compose` CLIs).

Quick start (recommended)
1. From the project root, build and start the services:

```bash
docker compose up --build
```

2. Open the app in your browser: http://localhost:5000/ (the Flask app serves the frontend).

What the compose file does
- MySQL service creates a `devops` database using `MYSQL_ROOT_PASSWORD` set to `root`.
- The Flask service is built from the local `Dockerfile` and connects to the `mysql` service using environment variables defined in `docker-compose.yml`.
- The Flask app exposes a `/health` endpoint used by the container healthcheck.

Environment (compose defaults)
- MySQL environment (in `docker-compose.yml`):
  - `MYSQL_ROOT_PASSWORD`: root
  - `MYSQL_DATABASE`: devops
- Flask environment (in `docker-compose.yml`):
  - `MYSQL_HOST`: mysql
  - `MYSQL_USER`: root
  - `MYSQL_PASSWORD`: root
  - `MYSQL_DB`: devops

Run the app locally (without Docker)
1. Create and activate a Python virtualenv.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Start the app:

```bash
python app.py
```

Then visit http://localhost:5000/.

Notes & troubleshooting
- The app will create the `messages` table on startup if it does not exist; `message.sql` is optional.
- Docker Compose `depends_on` in this repo is configured to wait for MySQL health; if you still see connection errors, wait a few seconds and retry or check the logs with:

```bash
docker compose logs -f
```

- If the Flask container healthcheck fails, inspect the Flask logs or try curling the endpoint inside the container:

```bash
docker compose exec flask-app python -c "import urllib.request; print(urllib.request.urlopen('http://localhost:5000/health').read())"
```


