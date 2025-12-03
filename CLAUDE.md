# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a lightweight Nginx webserver project using Docker Compose for containerization. The project provides a simple web server setup with Nginx running on an Alpine Linux container, with Cloudflare tunnel integration for external access.

## Architecture

- **Container Platform**: Docker Compose V2
- **Web Server**: Nginx (Alpine Linux based)
- **Networking**: Custom Docker network `webserver_network` with bridge driver
- **External Access**: Cloudflare tunnel (configured via nohup.out logs)
- **Volume Management**: External Docker volumes for Nginx configuration persistence

## Development Commands

### Docker Operations
```bash
# Start the webserver
docker compose up -d

# Stop the webserver
docker compose down

# View logs
docker compose logs -f webserver

# Rebuild and start
docker compose up -d --build
```

### Environment Configuration
- The webserver runs on port `${LARAVEL_PORT:-80}` (defaults to 80)
- Environment variables can be set in a `.env` file or exported in shell

### Cloudflare Tunnel
The project includes Cloudflare tunnel configuration for external access:
- Tunnel ID: `0506ea50-7fa0-477a-b0ee-16f16e9e1e13`
- Metrics server available on `127.0.0.1:20241/metrics`

## File Structure

- `compose.yaml`: Docker Compose configuration defining the webserver service
- `nginx/`: Nginx configuration directory (not present in repo, expected to be created)
  - `conf.d/`: Additional Nginx configurations
  - `main-config/`: Main Nginx configuration files
  - `sites-available/`: Available site configurations
  - `sites-enabled/`: Enabled site configurations
- `logs/nginx/`: Nginx log directory

## External Volumes

The project uses external Docker volumes that must be created manually:
- `nginx-main-config`: Main Nginx configuration
- `nginx-sites-available`: Available site configurations
- `nginx-sites-enabled`: Enabled site configurations

## Network Configuration

The project creates a custom Docker network `webserver_network` that can be referenced by other Docker Compose projects as an external network for service-to-service communication.

## Health Check

The webserver includes a health check that:
- Tests connectivity via wget to `http://localhost:${LARAVEL_PORT:-80}/`
- Runs every 30 seconds after a 30-second start period
- Allows 3 retries with 10-second timeout before marking as unhealthy