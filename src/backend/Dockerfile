FROM python:3.13-alpine as builder

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app

# Install dependencies in a cache layer. This speeds up build time
RUN --mount=type=cache,target=/root/.cache/uv \
  --mount=type=bind,source=uv.lock,target=uv.lock \
  --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
  uv sync --locked --no-install-project --no-editable

COPY . /app

# Sync the project and install in, now that we have access to the source code
RUN --mount=type=cache,target=/root/.cache/uv \
  uv sync --locked --no-editable # --no-dev

FROM python:3.13-alpine

# add the app user and group
RUN addgroup -S app && adduser -S -G app app
# Copy the environment, but not the source code
COPY --from=builder --chown=app:app /app/.venv /app/.venv

CMD ["/app/.venv/bin/study-tracker-api"]
