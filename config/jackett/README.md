# Jackett Configuration

## Overview
Jackett is a proxy server that translates queries from applications like Sonarr and Radarr into tracker-specific http queries and parses the html or json response back into results.

## What Jackett Does
- Provides a unified API for torrent indexers
- Supports hundreds of public and private trackers
- Handles tracker-specific authentication and search formats
- Works alongside or as an alternative to Prowlarr

## Configuration Directory
This directory stores:
- Indexer configurations and credentials
- Server settings and API keys
- Custom indexer definitions
- Search cache and logs

## First Time Setup
1. Access Jackett at: `http://localhost:9117`
2. Note the API Key from the top of the page
3. Add indexers by clicking "Add indexer"
4. Configure each indexer with credentials if needed
5. Test indexers to ensure they're working

## Integration with Sonarr/Radarr
Add Jackett indexers manually in Sonarr/Radarr:
- **URL**: `http://jackett:9117/api/v2.0/indexers/INDEXERNAME/results/torznab/`
- **API Key**: Copy from Jackett dashboard
- **Categories**: Use appropriate categories (5000 for TV, 2000 for Movies)

## vs Prowlarr
- **Jackett**: Manual indexer management, individual setup in each *arr app
- **Prowlarr**: Centralized management, automatic sync to *arr apps
- **Recommendation**: Use Prowlarr for easier management, Jackett for specialized needs

## Key Features
- **FlareSolverr Integration**: Handles CloudFlare-protected sites
- **Manual Search**: Test searches directly in Jackett interface
- **Detailed Logs**: Comprehensive logging for troubleshooting
- **Blackhole**: Can save .torrent files to directory

## Configuration Settings
- **FlareSolverr URL**: `http://flaresolverr:8191`
- **Base Path Override**: Leave empty unless using reverse proxy
- **API Key**: Auto-generated, used by Sonarr/Radarr

## Popular Public Indexers
- 1337x
- The Pirate Bay
- YTS
- EZTV
- TorrentGalaxy
- LimeTorrents

## Troubleshooting
```bash
# Check Jackett logs
docker logs jackett

# Test individual indexers
# Click "Manual Search" for each indexer in Jackett UI

# Check indexer status
# Look for red/yellow indicators in indexer list
```

## Note
If using Prowlarr, you may not need Jackett unless you have specific indexers that work better through Jackett's implementation.
