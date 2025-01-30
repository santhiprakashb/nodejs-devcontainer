# Build stage
FROM node:20-bullseye-slim

# Pre-configure certificates to avoid ARM64 issues
ENV NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt

# Install essential tools and dependencies in separate layers
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    openssh-client \
    procps \
    tar \
    curl \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Install development tools in a separate layer
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    python3 \
    g++ \
    make \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Install X11 and security dependencies in a separate layer
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    libx11-dev \
    libxkbfile-dev \
    libsecret-1-dev \
    && rm -rf /var/lib/apt/lists/*

# Handle certificates separately
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/* \
    && update-ca-certificates --fresh

# Upgrade npm to version 11
RUN npm install -g npm@11.0.0

# Set working directory
WORKDIR /app

# Create SSH directory and set permissions
RUN mkdir -p /home/node/.ssh && \
    chmod 700 /home/node/.ssh && \
    chown -R node:node /home/node/.ssh

# Create cursor server directory with proper permissions
RUN mkdir -p /home/node/.cursor-server && \
    chown -R node:node /home/node/.cursor-server && \
    chmod 755 /home/node/.cursor-server

# Switch to non-root user
USER node

# Create .npm directory for the node user
RUN mkdir -p /home/node/.npm

EXPOSE 3000

CMD ["npm", "start"]

