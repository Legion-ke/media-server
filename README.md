# Media Server Stack

This Docker Compose configuration sets up a complete media server stack with automated media management, indexing, and streaming capabilities.

## Services Included

- **Jellyfin**: Media streaming server (access on port 8096)
- **Radarr**: Movie collection manager (access on port 7878)
- **Sonarr**: TV series collection manager (access on port 8989)
- **Prowlarr**: Indexer manager and proxy (access on port 9696)
- **qBittorrent**: Torrent client with web interface (access on port 8080)
- **Jackett**: API support for your torrent trackers (access on port 9117)

## Prerequisites

- Docker and Docker Compose installed
- Sufficient storage space for media files
- A mount point at `/mnt/media/media` for your media files

## Installation

1. Create a directory for your media server:
   ```bash
   mkdir -p ~/mediaserver
   cd ~/mediaserver
   ```

2. Save the docker-compose.yml file in this directory

3. Create necessary directories for configuration:
   ```bash
   mkdir -p jellyfin/config radarr/config sonarr/config prowlarr/config qbittorrent/config jackett/config
   ```

4. Make sure your media directory exists:
   ```bash
   sudo mkdir -p /mnt/media/media/Downloads
   sudo chown -R 1000:1000 /mnt/media/media
   ```

5. Start the services:
   ```bash
   docker-compose up -d
   ```

## Ports

| Service      | Port  | Protocol    | Purpose                     |
|--------------|-------|-------------|----------------------------|
| Jellyfin     | 8096  | TCP         | Web interface              |
| Radarr       | 7878  | TCP         | Web interface              |
| Sonarr       | 8989  | TCP         | Web interface              |
| Prowlarr     | 9696  | TCP         | Web interface              |
| qBittorrent  | 8080  | TCP         | Web interface              |
| qBittorrent  | 6881  | TCP/UDP     | BitTorrent traffic         |
| Jackett      | 9117  | TCP         | Web interface              |

## Remote Access Configuration

To access your services remotely:

1. Set up port forwarding on your router for the services you want to access
2. Consider using a dynamic DNS service like DuckDNS if you don't have a static IP
3. For security, consider setting up a VPN or reverse proxy with authentication

## Initial Setup

After deployment, you'll need to:

1. Set up Jellyfin by navigating to http://your-server-ip:8096
2. Configure Radarr and Sonarr to use Prowlarr for indexers
3. Connect Radarr and Sonarr to qBittorrent for downloads
4. Configure Prowlarr with your preferred indexers

## Upgrading

To update your containers to the latest versions:

```bash
docker-compose pull
docker-compose up -d
```

## Maintenance

- Backup configuration directories regularly
- Monitor storage space
- Check logs with `docker-compose logs -f [service-name]`

## Troubleshooting

- If containers can't access media folders, check SELinux:
  ```bash
  sudo chcon -Rt svirt_sandbox_file_t /mnt/media/media
  ```
- Verify PUID and PGID match your user's ID:
  ```bash
  id $USER
  ```

## Notes

- All services use the host network mode for simplicity and compatibility
- DLNA discovery requires UDP ports to be open for local network discovery
- Consider using Prowlarr exclusively as it replaces Jackett's functionality
