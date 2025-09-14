# Home Media Server

A complete Docker-based media server stack with automated downloading, organization, and streaming capabilities.

## Services Included

- **Plex** - Media server for streaming your content
- **Sonarr** - TV show collection manager
- **Radarr** - Movie collection manager  
- **Prowlarr** - Indexer manager for both Sonarr and Radarr
- **Jackett** - Additional torrent indexer proxy
- **qBittorrent** - Download client (VPN-protected)
- **OpenVPN** - VPN client for secure downloading
- **FlareSolverr** - CloudFlare solver for protected indexers

## Quick Start

### 1. Configure Storage Paths

Copy the environment template and customize your storage paths:

```bash
cp .env.example .env
```

Edit `.env` to set your storage locations:

```env
# Customize these paths for your setup
MEDIA_ROOT=/path/to/your/media
DOWNLOADS_ROOT=/path/to/your/downloads
```

### 2. Create Required Directories

```bash
# Create media directories
mkdir -p $MEDIA_ROOT/{tv,movies,music}
mkdir -p $DOWNLOADS_ROOT

# Or with default paths:
sudo mkdir -p /mnt/WD_1T/media/{tv,movies,music}
sudo mkdir -p /mnt/WD_1T/downloads
sudo chown -R $USER:$USER /mnt/WD_1T/
```

### 3. Configure OpenVPN

Add your VPN configuration files to `./config/openvpn/`:

```bash
# Copy your VPN provider's .ovpn file and rename it
cp your-vpn-config.ovpn ./config/openvpn/vpn.conf

# Create authentication file
echo "your_username" > ./config/openvpn/vpn.auth
echo "your_password" >> ./config/openvpn/vpn.auth
chmod 600 ./config/openvpn/vpn.auth
```

### 4. Start the Stack

```bash
docker-compose up -d
```

## Service Access

| Service | URL | Purpose |
|---------|-----|---------|
| Plex | http://localhost:32400/web | Media streaming |
| Sonarr | http://localhost:8989 | TV show management |
| Radarr | http://localhost:7878 | Movie management |
| Prowlarr | http://localhost:9696 | Indexer management |
| Jackett | http://localhost:9117 | Additional indexers |
| qBittorrent | http://localhost:8080 | Download client |
| FlareSolverr | http://localhost:8191 | CloudFlare solver |

## Storage Configuration

The stack uses Docker volumes for flexible storage configuration:

### Default Paths
- Media: `/mnt/WD_1T/media/{tv,movies,music}`
- Downloads: `/mnt/WD_1T/downloads`

### Custom Paths
Modify the `.env` file to use different storage locations:

```env
# Example for external drive
MEDIA_ROOT=/Volumes/External Drive/media
DOWNLOADS_ROOT=/Volumes/External Drive/downloads

# Example for NAS
MEDIA_ROOT=/mnt/nas/media  
DOWNLOADS_ROOT=/mnt/nas/downloads
```

### Volume Structure
```
media/
├── tv/           # TV shows (Sonarr → Plex)
├── movies/       # Movies (Radarr → Plex)  
└── music/        # Music (→ Plex)

downloads/        # qBittorrent downloads
├── movies/       # Movie downloads
├── tv/           # TV downloads
└── torrents/     # .torrent files
```

## Initial Configuration

### 1. Configure Download Client (qBittorrent)
1. Access qBittorrent at http://localhost:8080
2. Default login: `admin` / check container logs for password
3. Create categories: `movies`, `tv`
4. Set download paths to `/downloads`

### 2. Configure Indexers (Prowlarr)
1. Access Prowlarr at http://localhost:9696
2. Add indexers (1337x, YTS, EZTV, etc.)
3. Add Applications:
   - Sonarr: `http://sonarr:8989`
   - Radarr: `http://radarr:7878`
4. Sync indexers to applications

### 3. Configure Media Management
1. **Sonarr** (http://localhost:8989):
   - Add download client: qBittorrent (`qbittorrent:8080`)
   - Set category: `tv`
   - Add TV shows

2. **Radarr** (http://localhost:7878):
   - Add download client: qBittorrent (`qbittorrent:8080`) 
   - Set category: `movies`
   - Add movies

### 4. Configure Plex
1. Access Plex at http://localhost:32400/web
2. Add libraries:
   - Movies: `/movies`
   - TV Shows: `/tv`
   - Music: `/music`

## Security Features

### VPN Protection
- qBittorrent traffic routes through OpenVPN
- Kill switch: downloads stop if VPN disconnects
- IP verification in qBittorrent logs

### Network Isolation
- qBittorrent bound to OpenVPN container
- Local network access maintained for other services
- Automatic DNS leak protection

## Monitoring

### Check VPN Status
```bash
# Verify VPN connection
docker logs openvpn

# Check qBittorrent's external IP
docker exec qbittorrent curl -s ifconfig.me

# Should match your VPN server's IP
```

### Service Logs
```bash
# View logs for any service
docker-compose logs [service-name]

# Follow logs in real-time
docker-compose logs -f [service-name]
```

## Backup

### Important Directories to Backup
- `./config/` - All service configurations
- `$MEDIA_ROOT/` - Your media library
- `.env` - Environment configuration

### Backup Command
```bash
# Create backup
tar -czf media-server-backup-$(date +%Y%m%d).tar.gz config/ .env

# Restore configuration
tar -xzf media-server-backup-*.tar.gz
```

## Troubleshooting

### Common Issues

**Services can't connect to qBittorrent:**
- Verify qBittorrent is accessible at `http://qbittorrent:8080`
- Check that OpenVPN container has network alias

**VPN not working:**
- Check OpenVPN logs: `docker logs openvpn`
- Verify VPN config files in `./config/openvpn/`
- Test VPN IP: `docker exec qbittorrent curl ifconfig.me`

**Permission issues:**
```bash
# Fix ownership
sudo chown -R 1000:1000 ./config/
sudo chown -R $USER:$USER $MEDIA_ROOT $DOWNLOADS_ROOT
```

**Storage path issues:**
- Verify paths in `.env` file exist
- Check directory permissions
- Restart containers: `docker-compose restart`

## Advanced Configuration

See individual service README files in `./config/` for detailed configuration options.

## Contributing

Feel free to submit issues and enhancement requests!
