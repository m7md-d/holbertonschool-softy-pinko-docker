# holbertonschool-softy-pinko-docker

A web app split into containers: an Nginx reverse proxy in front of an Nginx
static front-end and Flask API servers, wired together with Docker Compose. Each
`taskN` folder builds on the one before it, adding one piece of the stack.

## What this covers

| Concept | How it's used here |
|---------|--------------------|
| Docker images & Dockerfiles | Each service has a Dockerfile: base image, `RUN`/`COPY`/`CMD`, layers. `build` produces the image, `run` starts a container from it. |
| Docker Compose | One `docker-compose.yml` builds and starts every service at once. Services share an internal network and reach each other by name (`back-end`, `front-end`). |
| Reverse proxy with Nginx | A single Nginx entry point on port 80 routes `/` to the front-end and `/api` to the back-end. The inner services stay private — only the proxy is exposed. |
| Load balancing & scaling | Running two API servers with `--scale` lets Nginx + Docker DNS spread requests round-robin across them. |

## What each task adds

| Task | What it adds over the previous one |
|------|------------------------------------|
| task0 | First Docker image: `ubuntu`, APT update/upgrade, prints `Hello, World!`. |
| task1 | Back-end: installs Python3 + pip + Flask, runs `api.py` serving `GET /api/hello` on port 5252. |
| task2 | Front-end: moves the back-end into `back-end/`, adds an Nginx `front-end/` serving the static site on port 9000. |
| task3 | Connects them: front-end fetches `/api/hello` from the back-end, which enables `flask-cors` so the cross-origin call works. |
| task4 | Docker Compose: one `docker-compose.yml` builds and runs both services together instead of two manual commands. |
| task5 | Proxy: adds an Nginx proxy on port 80 routing `/` → front-end and `/api` → back-end; only the proxy is exposed to the host. |
| task6 | Scaling: runs two API servers with `docker-compose up --scale back-end=2`, load-balanced round-robin behind the proxy. |
