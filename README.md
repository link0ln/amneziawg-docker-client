# AmneziaWG Client Docker Setup

Docker container for AmneziaWG client with traffic obfuscation support.

## File Structure

```
.
├── docker-compose.yml      # Docker Compose configuration
├── Dockerfile              # Build image from sources
├── client.conf            # Your AmneziaWG config (create yourself)
└── client.conf.example    # Configuration example
```

## Quick Start

### 1. Create configuration file

```bash
cp client.conf.example client.conf
nano client.conf  # Edit for your parameters
```

### 2. Build and run the container

```bash
# Build image (may take several minutes)
docker-compose build

# Run container
docker-compose up -d

# View logs
docker-compose logs -f
```

### 3. Connection check

```bash
# Interface status
docker exec amneziawg-client wg show

# Check IP address
docker exec amneziawg-client ip addr show wg0

# Check external IP
curl ifconfig.me
```

## Management

```bash
# Stop container
docker-compose down

# Restart
docker-compose restart

# View logs
docker-compose logs -f amneziawg-client

# Rebuild after changes
docker-compose build --no-cache
docker-compose up -d
```

## Debugging

To enable verbose logging, modify in `docker-compose.yml`:

```yaml
environment:
  - LOG_LEVEL=debug
```

## Features

- **Network mode: host** - container uses host network stack
- **Automatic startup** - container restarts on system reboot
- **Traffic obfuscation** - support for all AmneziaWG parameters (Jc, Jmin, Jmax, S1, S2, H1-H4)

## Requirements

- Docker 20.10+
- Docker Compose 1.29+
- Linux kernel with TUN/TAP support

## Troubleshooting

### Container doesn't start

Check permissions:
```bash
chmod 600 client.conf
```

### VPN connects but no internet

Check AllowedIPs in config:
```
AllowedIPs = 0.0.0.0/0, ::/0
```

### DNS issues

Add to client.conf:
```
DNS = 1.1.1.1, 8.8.8.8
```
