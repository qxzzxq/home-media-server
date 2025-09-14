# Prowlarr Configuration

## Overview
Prowlarr is an indexer manager that centralizes and manages torrent and usenet indexers for Sonarr, Radarr, and other *arr applications.

## What Prowlarr Does
- Manages indexer configurations in one place
- Automatically syncs indexers to Sonarr and Radarr
- Provides unified search across all configured indexers
- Handles indexer authentication and rate limiting
- Monitors indexer health and statistics

## Configuration Directory
This directory stores:
- Database with indexer configurations
- Application sync settings
- Search history and statistics
- Custom definitions for indexers

## First Time Setup
1. Access Prowlarr at: `http://localhost:9696`
2. Go to Settings → Apps:
   - Add Sonarr: `http://sonarr:8989`
   - Add Radarr: `http://radarr:7878`
   - Use API keys from respective services
3. Go to Indexers → Add Indexer:
   - Add public trackers (1337x, RARBG alternatives, etc.)
   - Add private trackers if you have accounts
4. Test indexers and sync to applications

## Key Features
- **App Sync**: Automatically pushes indexer configs to Sonarr/Radarr
- **Categories**: Maps indexer categories to application categories
- **Priority**: Set indexer search priority and preferences
- **Health Checks**: Monitors indexer availability and response times

## Integration with Other Services
- **Sonarr**: Receives TV indexers automatically
- **Radarr**: Receives movie indexers automatically
- **FlareSolverr**: Handles CloudFlare-protected indexers

## Recommended Public Indexers
- YTS (movies)
- EZTV (TV shows)
- TorrentGalaxy
- 1337x
- LimeTorrents

## Configuration Tips
1. Enable categories that match your needs (TV/Movies)
2. Set reasonable search limits to avoid rate limiting
3. Configure FlareSolverr URL: `http://flaresolverr:8191`
4. Test each indexer after adding

## Troubleshooting
```bash
# Check Prowlarr logs
docker logs prowlarr

# Test indexer connectivity
# Go to Indexers → Test (green button)

# Check app sync status
# Go to Settings → Apps → Sync
```
