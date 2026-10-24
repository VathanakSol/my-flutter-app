FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Install basic tools needed by Flutter
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        ca-certificates \
        curl \
        git \
        jq \
        unzip \
        xz-utils \
    && rm -rf /var/lib/apt/lists/*

# Download and install the latest stable Flutter SDK
RUN set -eux; \
    RELEASE_JSON="$(curl -fsSL https://storage.googleapis.com/flutter_infra_release/releases/releases_linux.json)"; \
    BASE_URL="$(printf '%s' "$RELEASE_JSON" | jq -r '.base_url')"; \
    ARCHIVE="$(printf '%s' "$RELEASE_JSON" | jq -r '[.releases[] | select(.channel == "stable")] | sort_by(.release_date) | last | .archive')"; \
    curl -fsSL "${BASE_URL}/${ARCHIVE}" -o /tmp/flutter.tar.xz; \
    mkdir -p /opt; \
    tar -xf /tmp/flutter.tar.xz -C /opt; \
    rm -f /tmp/flutter.tar.xz; \
    chown -R vscode:vscode /opt/flutter

# Make Flutter available in PATH
ENV PATH="/opt/flutter/bin:${PATH}"

# Use the default devcontainer user
USER vscode

# Configure Flutter and pre-download web artifacts
RUN flutter config --no-analytics \
    && flutter config --enable-web \
    && flutter --version \
    && flutter precache --web