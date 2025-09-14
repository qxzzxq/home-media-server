# OpenVPN Setup Instructions

## Configuration Files Required

Place your VPN provider's configuration files in this directory (`./config/openvpn/`).

### Required Files:
1. **`vpn.conf`** - Your OpenVPN configuration file (rename from your provider's .ovpn file)
2. **`vpn.auth`** - Authentication file containing your username and password (one per line)

### Example vpn.auth file:
```
your_vpn_username
your_vpn_password
```

### File Permissions:
```bash
chmod 600 ./config/openvpn/vpn.auth
chmod 644 ./config/openvpn/vpn.conf
```

## Popular VPN Providers

### NordVPN:
- Download .ovpn files from: https://nordvpn.com/ovpn/
- Rename the downloaded file to `vpn.conf`
- Create `vpn.auth` with your NordVPN credentials

### ExpressVPN:
- Download .ovpn files from your account dashboard
- Rename to `vpn.conf`
- Create `vpn.auth` with your credentials

### Private Internet Access (PIA):
- Download .ovpn files from: https://www.privateinternetaccess.com/helpdesk/guides/desktop/windows/openvpn/openvpn-connection-ubuntu
- Rename to `vpn.conf`
- Create `vpn.auth` with your PIA credentials

### Surfshark:
- Download .ovpn files from your account
- Rename to `vpn.conf`
- Create `vpn.auth` with your Surfshark credentials

## Starting the Services

1. Place your VPN configuration files in this directory
2. Start the containers: `docker-compose up -d`
3. Check VPN connection: `docker logs openvpn`
4. Verify qBittorrent is using VPN: Check IP in qBittorrent web UI (http://localhost:8080)

## Troubleshooting

### Check VPN connection:
```bash
docker exec openvpn curl -s ifconfig.me
```

### Check qBittorrent network:
```bash
docker exec qbittorrent curl -s ifconfig.me
```

Both commands should return your VPN's IP address, not your real IP.

### View logs:
```bash
docker logs openvpn
docker logs qbittorrent
```

## Network Binding

qBittorrent is configured to use `network_mode: "service:openvpn"`, which means:
- All qBittorrent traffic goes through the OpenVPN container
- If VPN disconnects, qBittorrent loses internet access (kill switch effect)
- qBittorrent web UI is accessible via the OpenVPN container's ports

## Security Notes

- The `vpn.auth` file contains sensitive credentials - keep it secure
- The setup includes a kill switch - if VPN fails, torrenting stops
- Local network access is maintained via the `-r 192.168.0.0/16` route
