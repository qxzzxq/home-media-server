# Home Media Server Configuration Directories

This directory contains configuration folders for all services in the media server stack.

## Directory Structure

```
config/
├── jackett/        # Jackett torrent indexer configuration
├── openvpn/        # OpenVPN client configuration files
├── plex/           # Plex Media Server configuration
├── prowlarr/       # Prowlarr indexer manager configuration
├── qbittorrent/    # qBittorrent download client configuration
├── radarr/         # Radarr movie management configuration
└── sonarr/         # Sonarr TV show management configuration
```

## Service Information

### Plex (`./config/plex/`)
- **Port**: Host networking mode
- **Purpose**: Media server for streaming movies, TV shows, and music
- **First Setup**: Access via `http://localhost:32400/web` for initial configuration

### Sonarr (`./config/sonarr/`)
- **Port**: 8989
- **Purpose**: TV show collection manager and automatic downloader
- **Access**: `http://localhost:8989`

### Radarr (`./config/radarr/`)
- **Port**: 7878
- **Purpose**: Movie collection manager and automatic downloader
- **Access**: `http://localhost:7878`

### Prowlarr (`./config/prowlarr/`)
- **Port**: 9696
- **Purpose**: Indexer manager that feeds both Sonarr and Radarr
- **Access**: `http://localhost:9696`

### Jackett (`./config/jackett/`)
- **Port**: 9117
- **Purpose**: Torrent indexer proxy
- **Access**: `http://localhost:9117`

### qBittorrent (`./config/qbittorrent/`)
- **Port**: 8080 (via OpenVPN container)
- **Purpose**: Download client for torrents (VPN-protected)
- **Access**: `http://localhost:8080`
- **Note**: Bound to OpenVPN for secure downloading

### OpenVPN (`./config/openvpn/`)
- **Purpose**: VPN client to protect qBittorrent traffic
- **Required Files**: 
  - `vpn.conf` (your VPN provider's .ovpn file renamed)
  - `vpn.auth` (username and password, one per line)

## File Permissions

The directories are created with proper permissions for the LinuxServer.io containers:
- PUID=1000
- PGID=1000

## Backup Recommendations

These directories contain all your service configurations, databases, and settings. Consider backing up:
- `./config/` directory (entire folder)
- Important: `./config/plex/` contains your media library metadata
- `./config/sonarr/` and `./config/radarr/` contain your collection settings and download history

## First Run

1. Start services: `docker-compose up -d`
2. Configure each service through their web interfaces
3. Set up integrations between services (Prowlarr → Sonarr/Radarr → qBittorrent)
4. Add your VPN configuration to `./config/openvpn/`

## Troubleshooting

If you encounter permission issues:
```bash
sudo chown -R 1000:1000 ./config/
```

View service logs:
```bash
docker-compose logs [service-name]
```
