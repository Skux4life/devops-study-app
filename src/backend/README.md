uv sync --locked --no-editable
uv run study-tracker-api
TAG=00 && podman build --isolation chroot -t backend:00 .
 podman save -o backend00.tar localhost/backend:00
    6  trivy image --input backend00.tar
