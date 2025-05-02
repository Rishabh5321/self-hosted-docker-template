# My Personal Docker Media Server & Homelab Stack

This repository contains a `docker-compose.yml` file to deploy a comprehensive suite of media management, download, streaming, and utility services using Docker and Traefik as a reverse proxy.

## Overview

This setup uses Docker Compose to define and run multiple services in containers. Traefik handles routing traffic to the appropriate service based on hostname rules (e.g., `sonarr.yourdomain.com`), simplifying access and enabling potential future HTTPS via Let's Encrypt. Configuration is managed primarily through an `.env` file to keep sensitive information and environment-specific settings separate from the main compose file.

![Homelab Dashboard Screenshot](/.github/assessts/screenshot.png)
![Graphana Dashboard Screenshot](/.github/assessts/graphana.png)

## Services Included

* **Traefik:** Reverse proxy and load balancer, handling incoming requests and directing them to the correct service. Also provides a dashboard.
* **qBittorrent:** Popular BitTorrent client with a web UI.
* **Resilio Sync:** Peer-to-peer file synchronization tool.
* **Portainer CE:** Web UI for managing Docker containers, volumes, networks, etc.
* **Prowlarr:** Indexer manager for Sonarr, Radarr, etc., supporting various indexer types.
* **Jackett:** Indexer proxy/aggregator (can be used alongside or instead of Prowlarr).
* **Jellyfin:** Free and open-source media server for streaming movies, TV shows, music, etc.
* **Sonarr:** TV show download management and automation.
* **File Browser:** Web-based interface for Browse files on the server (use with caution!).
* **Watchtower:** Monitors running Docker containers and automatically updates them to the latest image.
* **Radarr:** Movie download management and automation.
* **Bazarr:** Companion application for Sonarr and Radarr to manage and download subtitles.
* **Jellyseerr:** Request management and media discovery tool for Jellyfin (and Plex).
* **Duplicati:** Backup client to store encrypted backups online or locally.
* **Tachidesk (Suwayomi):** A free and open source manga reader server.
* **Flaresolverr:** Proxy server designed to help bypass Cloudflare protection for web scraping/API access.
* **Free Games Claimer:** Automatically claims free games from GOG.com (Epic Games support commented out).
* **Syncify:** Syncs Spotify playlists/albums/artists to local music files.

## Prerequisites

* **Docker Engine:** Install Docker for your operating system.
* **Docker Compose:** Install Docker Compose (v2 syntax recommended).
* **Git:** (Optional) For cloning this repository.
* **Host Machine:** A Linux-based system is recommended for compatibility and features like hardware transcoding (`/dev/dri`).
* **Directory Structure:** You need to create the directories on your host machine that will be mounted as volumes. These paths are defined in the `.env` file.
* **`.env` File:** A correctly configured `.env` file is mandatory (see Configuration section).

## Configuration

This setup relies heavily on an `.env` file placed in the same directory as the `docker-compose.yml` file. **This file contains sensitive information and environment-specific paths and MUST NOT be committed to version control (e.g., Git).**

1.  **Create the `.env` file:** You can copy the example structure below or use `cp .env.example .env` if you create an example file.
2.  **Edit `.env`:** Fill in the values specific to your environment.

### `.env` File Structure Example (`.env.example`)

```dotenv
# --- General Settings ---
# Timezone (e.g., America/New_York, Europe/London, Asia/Kolkata)
TZ=Asia/Kolkata
# User and Group ID for file permissions. Find yours with `id $USER` on Linux.
PUID=1000
PGID=1000
# Base domain for accessing services via Traefik (e.g., yourdomain.com, home.local)
BASE_DOMAIN=server

# --- Base Paths (IMPORTANT: Adjust to your actual host system paths) ---
# Base directory for storing Docker application configurations
DOCKER_CONFIG_BASE=/mnt/Docker/Docker
# Base directory for other Docker data (if needed, otherwise combine with config base)
# DOCKER_DATA_BASE=/mnt/Docker
# Path for downloaded files (used by qBittorrent, Sonarr, Radarr, etc.)
MEDIA_DOWNLOADS_PATH=/mnt/Raid/Downloads
# Path(s) for your main media library (used by Jellyfin, Sonarr, Radarr, Bazarr, etc.)
MEDIA_LIBRARY_PATH_RAID=/mnt/Raid/Learn/Bonus
MEDIA_LIBRARY_PATH_EDISK=/mnt/E_Disk/Learn/Bonus
# Path for Duplicati backup destination
BACKUP_PATH=/mnt/Raid/Backup
# Path to host's /mnt directory (Used by File Browser, Duplicati - USE CAREFULLY!)
HOST_MNT_PATH=/mnt
# Path to host's user home directory (Used by File Browser - USE CAREFULLY!)
HOST_HOME_PATH=/home/your_user # Change 'your_user'
# Path for Traefik Let's Encrypt certificates (if used)
TRAEFIK_ACME_PATH=/mnt/Docker/Docker/Traefik_Acme

# --- Service Specific ---
# Portainer external volume name (must match volume definition at end of compose file)
PORTAINER_DATA_VOLUME=portainer_data
# Watchtower update schedule (cron format: Min Hour Day Month DayOfWeek)
WATCHTOWER_SCHEDULE="00 10 * * *"
# Watchtower API token (create a strong, unique token)
WATCHTOWER_API_TOKEN=YourSecureWatchtowerToken
# Watchtower Telegram Notification URL (replace with your Bot Token and Channel ID)
WATCHTOWER_TELEGRAM_URL=telegram://BOT_TOKEN@telegram/?channels=CHANNEL_ID
# Duplicati Settings Encryption Key (random string, keep safe)
DUPLICATI_SETTINGS_KEY=YourDuplicatiSettingsKey
# Duplicati Web UI Password (change this!)
DUPLICATI_PASSWORD=YourDuplicatiPassword
# GOG Credentials (replace with your actual credentials)
GOG_EMAIL=your_gog_email@example.com
GOG_PASSWORD=YourGogPassword!
# Free Games Claimer Notification URL (Telegram format)
FREE_GAMES_NOTIFY_URL=tgram://BOT_TOKEN/CHANNEL_ID
# Syncify Settings
SYNCIFY_THREAD_LIMIT=5
SYNCIFY_CROP_ALBUM_ART=false

# --- System Paths ---
# Path to Docker socket
DOCKER_SOCKET_PATH=/var/run/docker.sock
# Path to DRI device for hardware transcoding (Intel iGPU example)
DRI_DEVICE_PATH=/dev/dri

# --- Image Versions (Recommended: Use specific versions instead of 'latest' for stability) ---
TRAEFIK_VERSION=latest
QBITTORRENT_VERSION=latest
RESILIO_VERSION=latest
PORTAINER_VERSION=latest
PROWLARR_VERSION=latest
JACKETT_VERSION=latest
JELLYFIN_VERSION=latest
SONARR_VERSION=latest
FILEBROWSER_VERSION=latest
WATCHTOWER_VERSION=latest
RADARR_VERSION=latest
BAZARR_VERSION=latest
JELLYSEERR_VERSION=latest
DUPLICATI_VERSION=latest
TACHIDESK_VERSION=preview
FLARESOLVERR_VERSION=latest
FREE_GAMES_CLAIMER_VERSION=latest
SYNCIFY_VERSION=latest

```

Ensure the .env file is in the repo folder and `docker-compose.yaml` and `.env` file should have the same path

After that just do 

```
docker-compose up -d
```

And you containers are now ready to use.