# Plex Media Server Configuration

## Overview
Plex is a media server that organizes and streams your personal media collection (movies, TV shows, music, photos) to various devices.

## Configuration Directory
This directory contains all Plex configuration files, including:
- Database files
- Metadata and artwork cache
- Plugin configurations
- Transcoding settings
- User preferences

## First Time Setup
1. Start the container: `docker-compose up -d plex`
2. Access Plex at: `http://localhost:32400/web`
3. Follow the setup wizard to:
   - Create/sign in to Plex account
   - Add media libraries pointing to your mounted volumes:
     - Movies: `/movies`
     - TV Shows: `/tv`
     - Music: `/music`

## Important Notes
- Uses host networking for better performance and device discovery
- Hardware transcoding enabled via `/dev/dri` device mapping
- Media files are read-only mounted from `/mnt/WD_1T/media/`

## Useful Commands
```bash
# View Plex logs
docker logs plex

# Restart Plex
docker-compose restart plex

# Check Plex status
docker exec plex ps aux | grep Plex
```

## Backup
This directory contains your entire Plex configuration. Back it up regularly to preserve:
- Library metadata and artwork
- Watch history and ratings
- Custom collections and playlists
