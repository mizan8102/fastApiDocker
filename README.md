# FastAPI Docker Project Example

This project demonstrates how to containerize a FastAPI application with Postgres for development. Let's Encrypt for production deployment. The project structure includes Docker configuration files, a FastAPI application, and a Postgres database.

## Project Setup

1. **Create a project directory:**

    ```bash
    $ mkdir fastapi-docker-traefik && cd fastapi-docker-traefik
    $ python3.11 -m venv venv
    $ source venv/bin/activate
    ```

2. **Create the following files and folders:**

    ```plaintext
    ├── app
    │   ├── __init__.py
    │   └── main.py
    └── requirements.txt
    ```

3. **Add FastAPI and Uvicorn to `requirements.txt`:**

    ```plaintext
    fastapi==0.89.1
    uvicorn==0.20.0
    ```

4. **Install dependencies:**

    ```bash
    (venv)$ pip install -r requirements.txt
    ```

5. **Create a simple FastAPI application in `app/main.py`:**

    ```python
    # app/main.py

    from fastapi import FastAPI

    app = FastAPI(title="FastAPI, Docker, and Traefik")

    @app.get("/")
    def read_root():
        return {"hello": "world"}
    ```

6. **Run the application:**

    ```bash
    (venv)$ uvicorn app.main:app
    ```

    Open [http://127.0.0.1:8000](http://127.0.0.1:8000) to see the result.

7. **Build the Docker image:**

    ```bash
    $ docker build -t fastapi-docker-traefik .
    ```

8. **Create a `docker-compose.yml` file:**

    ```yaml
    # docker-compose.yml

    version: '3.8'

    services:
      web:
        build: .
        command: bash -c 'while !</dev/tcp/db/5432; do sleep 1; done; uvicorn app.main:app --host 0.0.0.0'
        volumes:
          - .:/app
        expose:
          - 8000
        environment:
          - DATABASE_URL=postgresql://fastapi_traefik:fastapi_traefik@db:5432/fastapi_traefik
        depends_on:
          - db
      db:
        image: postgres:15-alpine
        volumes:
          - postgres_data:/var/lib/postgresql/data/
        expose:
          - 5432

    volumes:
      postgres_data:
    ```

9. **Build the Docker image and run the containers:**

    ```bash
    $ docker-compose up -d --build
    ```

    Open [http://127.0.0.1:8000](http://127.0.0.1:8000) to see the FastAPI application running.

## [Details Link] (https://docs.google.com/document/d/1cewA8IJ9TPw-_InoyrFaXQsSRYnjNZstvwow5rtgikk/edit?usp=sharing)