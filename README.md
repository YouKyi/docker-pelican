[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/YouKyi/docker-pelican)
# Docker Pelican Panel

A Docker Compose setup for deploying [Pelican Panel](https://pelican.dev/) - a modern game server management panel. This repository provides two deployment configurations: with and without a reverse proxy.

## 📋 Features

* **Complete Stack Deployment**: Pelican Panel, Wings daemon, and MariaDB database
* **Two Configuration Options**:
  * Direct deployment with built-in Caddy server
  * Deployment behind an external reverse proxy
* **Monitoring Ready**: Includes Pelican exporter for metrics
* **Easy Configuration**: Simple environment variable setup
* **Production Ready**: Includes security headers and optimized settings

## 🚀 Quick Start

### Prerequisites

* Docker and Docker Compose installed
* A domain name (for production use)
* Basic understanding of Docker

## 📦 Installation

### Option 1: Without Reverse Proxy (Direct Deployment)

This configuration includes a Caddyfile for direct HTTPS deployment.


1. Navigate to the directory:

```bash
cd "Without-reverse-proxy"
```


2. Edit the `CaddyFile` and replace `<domain>` with your actual domain name
3. Configure the environment variables in `docker-compose.yml`:
   * `TZ` and `APP_TIMEZONE`: Your timezone
   * `APP_URL`: Your panel URL
   * `ADMIN_EMAIL`: Admin email address
   * `MYSQL_PASSWORD` and `MYSQL_ROOT_PASSWORD`: Database passwords
4. Start the stack:

```bash
docker-compose up -d
```

### Option 2: With Reverse Proxy

This configuration is designed for use behind an external reverse proxy (like Nginx Proxy Manager, Traefik, etc.). 


1. Navigate to the directory:

```bash
cd "With reverse proxy"
```


2. Configure the same environment variables as mentioned above
3. Configure your external reverse proxy to forward traffic to port 80 of the panel container
4. Start the stack:

```bash
docker-compose up -d
```

## ⚙️ Configuration Details

### Services

#### Pelican Panel

* **Port**: 80 (HTTP)
* **Image**: `ghcr.io/pelican-dev/panel:latest`
* **Purpose**: Main control panel interface 

#### Wings Daemon

* **Port**: 2022 (SFTP)
* **Image**: `ghcr.io/pelican-dev/wings:latest`
* **Purpose**: Server daemon for managing game servers 

#### Database

* **Image**: MariaDB latest
* **Database Name**: panel
* **User**: pelican 

#### Pelican Exporter

* **Port**: 9531
* **Purpose**: Prometheus metrics exporter 

### Security Features

The Caddyfile includes several security headers:

* Strict Transport Security (HSTS)
* Content Security Policy
* X-Frame-Options
* XSS Protection

### File Upload Configuration

Maximum upload size is set to 100MB for file uploads and PHP processing.

## 🔧 Post-Installation


1. Access your panel at your configured domain
2. Complete the initial setup wizard
3. Configure Wings through the admin panel
4. Add your first node and allocations

## 📊 Monitoring

Access Pelican metrics at `http://your-server:9531/metrics` for Prometheus integration.

## ⚠️ Important Notes

* **Change Default Passwords**: Make sure to change the default MySQL passwords before deployment
* **Domain Configuration**: For the "Without reverse proxy" option, edit the CaddyFile with your actual domain
* **Docker Socket**: Wings requires access to the Docker socket to manage game servers
* **Timezone**: Update timezone settings to match your location
* **Persistent Data**: All important data is stored in Docker volumes and will persist across container restarts

## 📝 Notes

This repository provides Docker Compose configurations for Pelican Panel deployment. There are two nearly identical configurations in separate directories - the main difference is that the "Without reverse proxy" version includes a Caddyfile for direct HTTPS deployment, while the "With reverse proxy" version is designed to work behind an external reverse proxy setup. Both configurations include the complete Pelican stack: Panel, Wings, MariaDB database, and Pelican exporter for monitoring.

Make sure to properly configure all environment variables marked with "Change this variable" comments before deploying to production.