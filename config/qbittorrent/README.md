# qBittorrent Configuration

## Overview
qBittorrent is a BitTorrent client that handles all torrent downloads for your media server. It's configured to route all traffic through the OpenVPN container for privacy and security.

## What qBittorrent Does
- Downloads torrents sent by Sonarr and Radarr
- Manages download queues and seeding
- Provides web-based management interface
- Organizes downloads by category

## Configuration Directory
This directory stores:
- qBittorrent settings and preferences
- RSS feed configurations
- Download categories and labels
- Speed limits and scheduling settings

## Network Configuration
**IMPORTANT**: This qBittorrent instance is bound to the OpenVPN container:
- All traffic routes through VPN for privacy
- If VPN disconnects, qBittorrent loses internet (kill switch)
- Accessible via OpenVPN container's network interface

## First Time Setup
1. Access qBittorrent at: `http://localhost:8080`
2. Default credentials:
   - Username: `admin`
   - Password: Check container logs: `docker logs qbittorrent`
3. Change default password immediately
4. Configure download paths:
   - Default Save Path: `/downloads`
   - Create categories for organization

## Recommended Categories
Create these categories for better organization:
- `movies` → `/downloads/movies`
- `tv` → `/downloads/tv`
- `music` → `/downloads/music`

## Integration with *arr Apps
Configure in Sonarr/Radarr:
- **Host**: `qbittorrent` (or `openvpn`)
- **Port**: `8080`
- **Username/Password**: As configured
- **Category**: `tv` for Sonarr, `movies` for Radarr

## Security Features
- **VPN Binding**: All traffic goes through OpenVPN
- **Kill Switch**: No traffic leaks if VPN fails
- **IP Verification**: Check current IP in qBittorrent logs

## Key Settings to Configure
1. **Downloads**:
   - Default Save Path: `/downloads`
   - Automatically add torrents from: `/downloads/watch`
   - Copy .torrent files to: `/downloads/torrents`

2. **Connection**:
   - Port: `6881` (already configured in OpenVPN)
   - Use UPnP/NAT-PMP: Disabled (not needed with VPN)

3. **Speed**:
   - Set upload/download limits based on your connection
   - Enable scheduling if needed

4. **BitTorrent**:
   - Privacy: Enable anonymous mode
   - Seeding Limits: Configure based on tracker requirements

## Monitoring VPN Connection
Check that qBittorrent is using VPN IP:
```bash
# Check qBittorrent's external IP
docker exec qbittorrent curl -s ifconfig.me

# Check OpenVPN logs
docker logs openvpn

# Check qBittorrent logs for IP detection
docker logs qbittorrent | grep "external IP"
```

## File Organization
Downloads are saved to `/downloads` which maps to `/mnt/WD_1T/downloads/`:
```
/downloads/
├── movies/          # Movie downloads (Radarr category)
├── tv/             # TV downloads (Sonarr category)
├── torrents/       # .torrent files backup
└── watch/          # Auto-add torrent directory
```

## Troubleshooting
```bash
# Check if qBittorrent can reach the internet
docker exec qbittorrent ping 8.8.8.8

# Verify VPN connection
docker exec openvpn curl -s ifconfig.me

# Check container connectivity
docker exec sonarr ping qbittorrent
```
