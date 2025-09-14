# Sonarr Configuration

## Overview
Sonarr is an automated TV show collection manager that monitors, downloads, and organizes your TV series.

## What Sonarr Does
- Monitors TV show release schedules
- Searches for episodes using indexers (Prowlarr/Jackett)
- Sends download requests to qBittorrent
- Renames and organizes downloaded episodes
- Manages episode quality and upgrades

## Configuration Directory
This directory stores:
- Database with show information and download history
- Quality profiles and naming conventions
- Indexer and download client configurations
- Custom scripts and notifications

## First Time Setup
1. Access Sonarr at: `http://localhost:8989`
2. Go to Settings → Download Clients:
   - Add qBittorrent client
   - Host: `qbittorrent` (or `openvpn`)
   - Port: `8080`
   - Category: `tv` (recommended)
3. Go to Settings → Indexers:
   - Add Prowlarr indexers or configure Jackett
4. Add TV shows via the "Add Series" button

## Key Settings
- **Media Management**: Configure file naming and folder structure
- **Quality Profiles**: Set preferred video quality (1080p, 4K, etc.)
- **Release Profiles**: Fine-tune release preferences and restrictions

## Integration with Other Services
- **Prowlarr**: Provides indexers automatically
- **qBittorrent**: Downloads episodes via VPN
- **Plex**: Automatically detects new episodes in `/tv` folder

## Useful Paths
- TV Library: `/tv` (mounted to `/mnt/WD_1T/media/tv`)
- Downloads: `/downloads` (mounted to `/mnt/WD_1T/downloads`)

## Troubleshooting
```bash
# Check Sonarr logs
docker logs sonarr

# Test download client connection
# Go to Settings → Download Clients → Test
```
