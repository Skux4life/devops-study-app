# DevOps Study App Frontend

uv sync --locked --no-editable
uv run study-tracker-frontend
TAG=00 && docker build -t frontend:00 .
