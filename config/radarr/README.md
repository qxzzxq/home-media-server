# Radarr Configuration

## Overview
Radarr is an automated movie collection manager that monitors, downloads, and organizes your movie library.

## What Radarr Does
- Monitors movie releases and availability
- Searches for movies using indexers (Prowlarr/Jackett)
- Sends download requests to qBittorrent
- Renames and organizes downloaded movies
- Manages movie quality and upgrades

## Configuration Directory
This directory stores:
- Database with movie information and download history
- Quality profiles and naming conventions
- Indexer and download client configurations
- Custom scripts and notifications

## First Time Setup
1. Access Radarr at: `http://localhost:7878`
2. Go to Settings → Download Clients:
   - Add qBittorrent client
   - Host: `qbittorrent` (or `openvpn`)
   - Port: `8080`
   - Category: `movies` (recommended)
3. Go to Settings → Indexers:
   - Add Prowlarr indexers or configure Jackett
4. Add movies via the "Add Movies" button

## Key Settings
- **Media Management**: Configure file naming and folder structure
- **Quality Profiles**: Set preferred video quality (1080p, 4K, HDR, etc.)
- **Custom Formats**: Advanced quality and release group preferences
- **Lists**: Import movies from IMDb, TMDb, or other lists

## Integration with Other Services
- **Prowlarr**: Provides indexers automatically
- **qBittorrent**: Downloads movies via VPN
- **Plex**: Automatically detects new movies in `/movies` folder

## Useful Paths
- Movie Library: `/movies` (mounted to `/mnt/WD_1T/media/movies`)
- Downloads: `/downloads` (mounted to `/mnt/WD_1T/downloads`)

## Movie Organization
Radarr will organize movies into folders like:
```
/movies/
├── Movie Title (2023)/
│   └── Movie Title (2023) - 1080p.mkv
└── Another Movie (2022)/
    └── Another Movie (2022) - 4K.mkv
```

## Troubleshooting
```bash
# Check Radarr logs
docker logs radarr

# Test download client connection
# Go to Settings → Download Clients → Test
```
