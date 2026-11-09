FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

RUN apt-get update && apt-get install -y \
      podman podman-compose fuse-overlayfs uidmap slirp4netns passt libcap2-bin \
    && rm -rf /var/lib/apt/lists/*

# Subordinate UID/GID ranges for nested rootless. Keep them inside 0-65535,
# because that's all the outer rootless container has mapped.
RUN echo "vscode:1001:64535" > /etc/subuid \
 && echo "vscode:1001:64535" > /etc/subgid

# Use file capabilities instead of setuid, which is what fixed the
# "newuidmap: write to uid_map failed: Operation not permitted" error.
RUN chmod 0755 /usr/bin/newuidmap /usr/bin/newgidmap \
 && setcap cap_setuid=ep /usr/bin/newuidmap \
 && setcap cap_setgid=ep /usr/bin/newgidmap

COPY --from=jdxcode/mise /usr/local/bin/mise /usr/local/bin/

# make sure mise is activated in both zsh and bash. Might be overridden by a user's dotfiles.
RUN echo 'eval "$(mise activate bash)"' >> /home/vscode/.bashrc && \
  echo 'eval "$(mise activate zsh)"' >> /home/vscode/.zshrc
