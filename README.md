# Docker Media Server Setup

This repository contains a Docker Compose configuration for a self-hosted media server. It includes services for torrenting, media management, automation, and more. Below is a detailed explanation of each service and how to configure and use them securely.

---

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Services Overview](#services-overview)
3. [Configuration](#configuration)
4. [Security Best Practices](#security-best-practices)
5. [Usage](#usage)
6. [Troubleshooting](#troubleshooting)
7. [Contributing](#contributing)
8. [License](#license)

---

## Prerequisites
Before using this setup, ensure you have the following:
- **Docker** and **Docker Compose** installed on your system.
- A basic understanding of Docker and containerization.
- A server or machine with sufficient resources (CPU, RAM, and storage).
- A domain name (optional but recommended for external access).

---

## Services Overview

### 1. qBittorrent
- **Purpose**: A BitTorrent client for downloading files.
- **Ports**: `8080` (Web UI), `6881` (Torrenting).
- **Volumes**:
  - `./config/qbittorrent`: Configuration files.
  - `./downloads`: Downloaded files.
- **Environment**:
  - `PUID`, `PGID`: User and group IDs for file permissions.
  - `TZ`: Timezone (e.g., `Asia/Kolkata`).

---

### 2. Portainer
- **Purpose**: A web-based Docker management tool.
- **Ports**: `8000`, `9443`, `9000`.
- **Volumes**:
  - `/var/run/docker.sock`: Docker socket access.
  - `portainer_data`: Persistent data.
- **Environment**:
  - `PUID`, `PGID`, `TZ`: Standard configuration.

---

### 3. Prowlarr
- **Purpose**: Indexer manager for Sonarr, Radarr, and Lidarr.
- **Ports**: `9696`.
- **Volumes**:
  - `./config/prowlarr`: Configuration files.

---

### 4. Jackett
- **Purpose**: Proxy server for torrent trackers.
- **Ports**: `9117`.
- **Volumes**:
  - `./config/jackett`: Configuration files.
  - `./downloads`: Shared downloads folder.

---

### 5. Jellyfin
- **Purpose**: Media server for streaming movies, TV shows, and music.
- **Ports**: `8096`, `8920`, `7359/udp`.
- **Volumes**:
  - `./config/jellyfin`: Configuration files.
  - `./media`: Media files.
- **Devices**:
  - `/dev/dri`: GPU passthrough for hardware acceleration.

---

### 6. Sonarr
- **Purpose**: TV show management and automation.
- **Ports**: `8989`.
- **Volumes**:
  - `./config/sonarr`: Configuration files.
  - `./media`: Media files.
  - `./downloads`: Shared downloads folder.

---

### 7. Filebrowser
- **Purpose**: Web-based file manager.
- **Ports**: `700`.
- **Volumes**:
  - `./data`: Managed files.
  - `./config/filebrowser`: Configuration files.

---

### 8. Watchtower
- **Purpose**: Automatically updates Docker containers.
- **Ports**: `2800`.
- **Environment**:
  - `WATCHTOWER_NOTIFICATIONS`: Notifications via Telegram.
  - `WATCHTOWER_CLEANUP`: Cleans up old images.

---

### 9. Radarr
- **Purpose**: Movie management and automation.
- **Ports**: `7878`.
- **Volumes**:
  - `./config/radarr`: Configuration files.
  - `./media`: Media files.
  - `./downloads`: Shared downloads folder.

---

### 10. Bazarr
- **Purpose**: Subtitle management for media.
- **Ports**: `6767`.
- **Volumes**:
  - `./config/bazarr`: Configuration files.
  - `./media`: Media files.

---

### 11. Homepage
- **Purpose**: A dashboard for managing services.
- **Ports**: `2500`.
- **Volumes**:
  - `./config/homepage`: Configuration files.
  - `/var/run/docker.sock`: Docker socket access.

---

### 12. Jellyseerr
- **Purpose**: A request management tool for Jellyfin.
- **Ports**: `5055`.
- **Volumes**:
  - `./config/jellyseerr`: Configuration files.

---

### 13. Duplicati
- **Purpose**: Backup tool for data.
- **Ports**: `8200`.
- **Environment**:
  - `SETTINGS_ENCRYPTION_KEY`: Encryption key for backups.
  - `DUPLICATI__WEBSERVICE_PASSWORD`: Web UI password.

---

### 14. Tailscale
- **Purpose**: VPN for secure access to services.
- **Environment**:
  - `TS_AUTH_KEY`: Tailscale authentication key.
  - `TS_ROUTES`: Network routes.

---

### 15. Plex
- **Purpose**: Media server for streaming.
- **Environment**:
  - `PLEX_CLAIM`: Plex claim token.
- **Devices**:
  - `/dev/dri`: GPU passthrough for hardware acceleration.

---

### 16. Dockerproxy
- **Purpose**: Proxy for Docker socket access.
- **Ports**: `127.0.0.1:2375`.
- **Volumes**:
  - `/var/run/docker.sock`: Docker socket access.

---

### 17. Watchstate
- **Purpose**: Tracks watched media.
- **Ports**: `8500`, `8600`.
- **Volumes**:
  - `./config/watchstate`: Configuration files.

---

### 18. Suwayomi
- **Purpose**: Manga reader and manager.
- **Ports**: `4567`.
- **Volumes**:
  - `./config/tachiyomi`: Configuration files.

---

### 19. Flaresolverr
- **Purpose**: Solves Cloudflare challenges for scraping.
- **Ports**: `8191`.

---

### 20. Free-Games-Claimer
- **Purpose**: Claims free games from platforms like Epic Games and GOG.
- **Environment**:
  - `NOTIFY`: Telegram notifications.

---

### 21. Nginx
- **Purpose**: Reverse proxy for external access.
- **Ports**: `80`, `443`.
- **Volumes**:
  - `./config/nginx`: Configuration files.

---

### 22. SpotDL
- **Purpose**: Downloads music from Spotify.
- **Ports**: `8800`.
- **Volumes**:
  - `./media/music`: Downloaded music.

---

### Configuration
1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo

2. Start the services:
   ```bash
   docker-compose up -d

### Security Best Practices

  Use a reverse proxy (e.g., nginx) with HTTPS for external access.
  Avoid exposing unnecessary ports to the internet.
  Use strong passwords and encryption keys.
  Regularly update Docker images and dependencies.

## Usage

1. **Access Services**:
   - Services can be accessed via their respective ports. For example:
     - qBittorrent: `http://localhost:8080`
     - Portainer: `http://localhost:9000`
     - Jellyfin: `http://localhost:8096`
   - Use a reverse proxy (e.g., `nginx`) for secure external access.

2. **Manage Containers**:
   - Use **Portainer** (`http://localhost:9000`) to manage Docker containers.

3. **Central Dashboard**:
   - Use **Homepage** (`http://localhost:2500`) as a central dashboard to monitor and access all services.

---

## Troubleshooting

1. **Check Container Logs**:
   To debug issues, check the logs of a specific container:
   ```bash
   docker logs <container_name>
