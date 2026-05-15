# ouroboros

> Automatically update your running Docker containers to the latest available image.

A fork of [Q00/ouroboros](https://github.com/Q00/ouroboros) with additional features and bug fixes.

## Features

- 🐳 Automatically updates running Docker containers
- 🔔 Notifications via multiple providers (Slack, Discord, Email, etc.)
- 📊 Prometheus metrics endpoint
- 🕐 Configurable polling interval
- 🏷️ Label-based container filtering
- 🔒 Support for private registries

## Quick Start

### Docker

```bash
docker run -d \
  --name ouroboros \
  -v /var/run/docker.sock:/var/run/docker.sock \
  ghcr.io/yourusername/ouroboros:latest
```

### Docker Compose

```yaml
version: '3'
services:
  ouroboros:
    image: ghcr.io/yourusername/ouroboros:latest
    container_name: ouroboros
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - INTERVAL=300
      - LOG_LEVEL=info
      - SELF_UPDATE=true
    restart: unless-stopped
```

## Configuration

Configuration is done via environment variables. Copy `.env.example` to `.env` and adjust as needed.

| Variable | Default | Description |
|----------|---------|-------------|
| `INTERVAL` | `300` | Polling interval in seconds |
| `LOG_LEVEL` | `info` | Logging level (debug, info, warn, error) |
| `SELF_UPDATE` | `false` | Allow ouroboros to update itself |
| `CLEANUP` | `false` | Remove old images after update |
| `MONITOR` | `all` | Comma-separated list of containers to monitor |
| `IGNORE` | `` | Comma-separated list of containers to ignore |
| `LABEL_ENABLE` | `false` | Only update containers with `ouroboros.enable=true` label |
| `REPO_USER` | `` | Registry username |
| `REPO_PASS` | `` | Registry password |
| `NOTIFIERS` | `` | Notification URLs (apprise format) |
| `METRICS_PORT` | `8080` | Prometheus metrics port |

## Notifications

Outroboros uses [Apprise](https://github.com/caronc/apprise) for notifications. Set the `NOTIFIERS` environment variable to a comma-separated list of notification URLs.

Examples:
- Slack: `slack://tokenA/tokenB/tokenC/`
- Discord: `discord://webhook_id/webhook_token/`
- Email: `mailto://user:password@gmail.com`

## Metrics

Prometheus metrics are available at `http://localhost:8080/metrics` by default.

Available metrics:
- `ouroboros_containers_updated_total` - Total number of container updates
- `ouroboros_containers_scanned_total` - Total number of containers scanned
- `ouroboros_errors_total` - Total number of errors encountered

## Development

### Prerequisites

- Python 3.11+
- Docker

### Setup

```bash
git clone https://github.com/yourusername/ouroboros.git
cd ouroboros
pip install -r requirements.txt
cp .env.example .env
```

### Running locally

```bash
python -m ouroboros
```

### Running tests

```bash
pytest tests/
```

## Contributing

Pull requests are welcome! Please check existing issues before opening a new one.

## License

MIT
