# Media Stack

A comprehensive, self-hosted media management and streaming solution built with Docker. This stack includes everything you need to manage, stream, and automate your media library with a focus on reliability, automation, and a modern user experience.

## 🎯 Overview

This media stack combines industry-standard tools for media management with custom solutions to create a complete entertainment server. It includes media servers, download clients, automation tools, monitoring, authentication, and a custom music streaming platform.

## 🚀 Features

### Core Capabilities
- **📺 Media Streaming** - Jellyfin for movies, TV shows, and music
- **🎵 Music Platform** - Lidify, a custom music streaming platform with AI-powered features
- **⬇️ Automated Downloads** - qBittorrent instances for movies, TV, and music
- **🤖 Smart Automation** - Sonarr, Radarr, and Lidarr for automated media management
- **🔍 Indexer Management** - Prowlarr for unified torrent/NZB indexer management
- **📊 Monitoring & Alerts** - Uptime Kuma for service monitoring
- **🔐 Authentication** - Authelia for SSO and 2FA
- **🌐 Reverse Proxy** - Caddy for automatic HTTPS and routing
- **📦 Container Management** - Portainer for easy Docker management

### Advanced Features
- **Auto-healing** - Automatically restarts unhealthy containers
- **Auto-updates** - Watchtower keeps containers up to date
- **Dynamic DNS** - Cloudflare DDNS integration
- **Subtitle Management** - Bazarr for automated subtitle downloads
- **P2P Sharing** - Soulseek for music discovery and sharing
- **AI Audio Analysis** - Machine learning-powered music analysis and recommendations

## 📋 Services

### Media Servers
- **Jellyfin** (`:8096`) - Open-source media server
- **Lidify Frontend** (`:3030`) - Custom music streaming UI
- **Lidify Backend** (`:3006`) - Music streaming API

### Download Clients
- **qBittorrent Movies** (`:8081`) - Dedicated movie torrents
- **qBittorrent TV** (`:8082`) - Dedicated TV show torrents  
- **qBittorrent Music** (`:8083`) - Dedicated music torrents
- **Lidify qBittorrent** (`:8084`) - Music torrents for Lidify
- **Soulseek** (`:5030`) - P2P music sharing

### Automation (*arr Stack)
- **Sonarr** (`:8989`) - TV show management
- **Radarr** (`:7878`) - Movie management
- **Lidarr** (`:8686`) - Music management
- **Prowlarr** (`:9696`) - Indexer management
- **Lidify Prowlarr** (`:9697`) - Indexer management for Lidify
- **Bazarr** (`:6767`) - Subtitle management
- **Recyclarr** - Automated quality profile sync

### Request Management
- **Jellyseerr** (`:5055`) - Media request and discovery platform

### Infrastructure
- **Caddy** (`:80`, `:443`) - Reverse proxy with auto-HTTPS
- **Authelia** (`:9091`) - Authentication and SSO
- **Homarr** (`:7575`) - Dashboard
- **Portainer** (`:9000`) - Container management
- **Uptime Kuma** (`:3001`) - Monitoring and alerts
- **Watchtower** - Automated container updates
- **Autoheal** - Automatic container recovery
- **Cloudflare DDNS** - Dynamic DNS updates

### Lidify Backend Services
- **PostgreSQL** (`:5433`) - Database with vector extensions
- **Redis** (`:6380`) - Caching and job queues
- **Audio Analyzer (Essentia)** - AI-powered music analysis
- **Audio Analyzer (CLAP)** - Advanced audio embeddings

## 🛠️ Installation

### Prerequisites
- Docker Engine 20.10+
- Docker Compose V2
- 4GB+ RAM recommended
- Storage for media files

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/AntonyBrown24/media-stack.git
   cd media-stack
   ```

2. **Create environment file**
   ```bash
   cp .env.example .env
   ```

3. **Edit the `.env` file with your configuration**
   - Set secure passwords and secrets
   - Configure Cloudflare credentials (if using DDNS)
   - Adjust timezone if needed

4. **Create required directories**
   ```bash
   mkdir -p config/{jellyfin,sonarr,radarr,lidarr,prowlarr,bazarr,recyclarr,qbittorrent-movies,qbittorrent-tv,qbittorrent-music,jellyseerr,homarr,caddy,authelia,soulseek,cloudflare-ddns,portainer,uptime-kuma}
   mkdir -p downloads/{movies,tv,music}
   mkdir -p media/{movies,tv,music}
   mkdir -p transcode
   ```

5. **Create external volume for Homarr**
   ```bash
   docker volume create homarr_data
   ```

6. **Start the stack**
   ```bash
   docker compose up -d
   ```

### Post-Installation

1. **Configure Prowlarr**
   - Access Prowlarr at `http://localhost:9696`
   - Add indexers
   - Connect to Sonarr, Radarr, and Lidarr

2. **Configure Download Clients**
   - Configure qBittorrent instances
   - Add to Sonarr, Radarr, and Lidarr

3. **Configure Media Libraries**
   - Set up Jellyfin libraries
   - Configure Sonarr/Radarr/Lidarr root folders
   - Add media to watch

4. **Set up Authentication (Optional)**
   - Configure Authelia for SSO
   - Set up 2FA

5. **Configure Monitoring**
   - Set up Uptime Kuma monitors for all services
   - Configure notification channels

## 📁 Directory Structure

```
media-stack/
├── docker-compose.yml          # Main stack configuration
├── .env.example               # Environment variables template
├── .env                       # Your environment variables (not in git)
├── config/                    # Application configurations
│   ├── jellyfin/
│   ├── sonarr/
│   ├── radarr/
│   ├── lidarr/
│   ├── prowlarr/
│   ├── bazarr/
│   ├── qbittorrent-*/
│   ├── caddy/
│   ├── authelia/
│   └── ...
├── downloads/                 # Download directories
│   ├── movies/
│   ├── tv/
│   └── music/
├── media/                     # Media libraries
│   ├── movies/
│   ├── tv/
│   └── music/
└── transcode/                 # Transcode cache
```

## 🔧 Configuration

### Environment Variables

All sensitive configuration is managed through environment variables in the `.env` file:

- **Homarr**: Dashboard encryption key
- **Cloudflare**: API token, Zone ID, update interval
- **Lidify**: Database credentials, session secrets, API keys
- **General**: Timezone settings

### Resource Limits

Download clients have resource limits to prevent system overload:
- CPU: 1-2 cores per instance
- RAM: 1-2GB per instance

Adjust these in the `docker-compose.yml` file based on your system capacity.

### Custom Domains

To use custom domains:
1. Configure Caddy with your domain in `config/caddy/Caddyfile`
2. Set up Cloudflare DDNS with your credentials
3. Configure Authelia for SSO access

## 🔄 Updates

The stack includes Watchtower for automatic updates. To manually update:

```bash
docker compose pull
docker compose up -d
```

## 🛡️ Security

### Best Practices
- Change all default passwords
- Use strong, unique secrets for encryption keys
- Enable 2FA in Authelia
- Keep services behind reverse proxy
- Regular backups of config directories
- Monitor with Uptime Kuma

### Network Isolation
Services are isolated in Docker networks:
- `media_net` - Main media stack network
- `lidify_network` - Lidify services network

## 📊 Monitoring

Access Uptime Kuma at `:3001` to monitor:
- Service availability
- Response times
- SSL certificate expiry
- Disk space usage

Configure notifications for:
- Slack, Discord, Telegram, Email
- Custom webhooks

## 🎵 Lidify - Music Streaming Platform

Lidify is a custom music streaming platform with advanced features:

### Features
- AI-powered music recommendations
- Audio analysis using Essentia and CLAP models
- Vector-based music similarity
- Automated music library management
- Integration with Lidarr and qBittorrent
- Modern React frontend

### Architecture
- **Frontend**: Next.js with React
- **Backend**: Node.js with Express
- **Database**: PostgreSQL with pgvector
- **Cache**: Redis
- **AI Services**: Python-based audio analyzers

### Resource Requirements
- Audio analyzers can use up to 6GB RAM
- GPU acceleration recommended for faster analysis (optional)

## 🐛 Troubleshooting

### Common Issues

**Container won't start**
- Check logs: `docker compose logs <service-name>`
- Verify environment variables are set
- Ensure required directories exist

**Permission errors**
- Verify PUID/PGID (default: 1000)
- Check directory ownership: `chown -R 1000:1000 config/`

**Network issues**
- Ensure ports aren't already in use
- Check firewall settings
- Verify Docker networks: `docker network ls`

**Database connection issues**
- Wait for health checks to pass
- Check database credentials in `.env`
- Review database logs

## 🤝 Contributing

This is a personal media stack, but feel free to:
- Fork the repository
- Suggest improvements
- Report issues
- Share your customizations

## 📝 License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

This stack is built on top of excellent open-source projects:
- [Jellyfin](https://jellyfin.org/)
- [Sonarr](https://sonarr.tv/), [Radarr](https://radarr.video/), [Lidarr](https://lidarr.audio/)
- [Prowlarr](https://prowlarr.com/)
- [qBittorrent](https://www.qbittorrent.org/)
- [Caddy](https://caddyserver.com/)
- [Authelia](https://www.authelia.com/)
- [Homarr](https://homarr.dev/)
- [Uptime Kuma](https://github.com/louislam/uptime-kuma)
- And many more!

## 📧 Contact

Created by AntonyBrown24

---

**⚠️ Disclaimer**: This stack is for personal use. Ensure you comply with local laws regarding media sharing and downloading.
