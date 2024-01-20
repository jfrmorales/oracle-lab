### README.md

---

## Overview

This configuration sets up a private Docker network with three services: Unbound, WireGuard, and AdGuard. Each service runs in its own container and has a specific role in managing network traffic and providing security and privacy features.

### Version
Docker Compose Version: "3"

### Network Configuration

- **Network Name**: `private_network`
- **Subnet**: `10.2.0.0/24`

### Services

#### 1. Unbound (DNS Resolver)
- **Image**: `pedantic/unbound:latest`
- **Container Name**: `unbound`
- **Hostname**: `unbound`
- **IP Address**: `10.2.0.200`
- **Configuration**:
  - Persistent configuration storage through a volume mounted at `./unbound:/opt/unbound/etc/unbound/`
  - Restarts automatically unless stopped manually.

#### 2. WireGuard (VPN Service)
- **Image**: `linuxserver/wireguard`
- **Container Name**: `wireguard`
- **IP Address**: `10.2.0.3`
- **Configuration**:
  - Depends on `unbound` and `adguard`.
  - Access to network administrative capabilities via `NET_ADMIN` and `SYS_MODULE`.
  - Environmental variables for user/group IDs, timezone, server port, and other optional settings.
  - Persistent storage for configuration and module compatibility.
  - Port `51820` exposed for UDP traffic.
  - Custom sysctls configuration.
  - Restarts automatically unless stopped manually.

#### 3. AdGuard (Network-wide Ads & Tracking Blocker)
- **Image**: `adguard/adguardhome`
- **Container Name**: `adguard`
- **Hostname**: `adguard`
- **IP Address**: `10.2.0.100`
- **Configuration**:
  - Depends on `unbound`.
  - Persistent storage for work and configuration data.
  - Restarts automatically unless stopped manually.

### Deployment Instructions

1. Ensure Docker and Docker Compose are installed on your system.
2. Save the Docker Compose YAML configuration to a file named `docker-compose.yml`.
3. Navigate to the directory containing your `docker-compose.yml`.
4. Run the command `docker-compose up -d` to start the services in detached mode.
5. Monitor the logs or use `docker-compose ps` to verify that the services are running correctly.

### Notes
- Make sure to adjust the timezone for the WireGuard service (`TZ=Europe/Madrid`) to match your local timezone.
- Uncomment and configure the `SERVERURL` line in WireGuard service if you're using DDNS.
- Ensure the volumes paths exist on your host or are correctly mounted for persistent data storage.
- Review and adjust firewall and port forwarding settings on your network to allow traffic to the WireGuard port (`51820/udp`).

For more detailed information on each service's configuration and usage, consult the official documentation for [Unbound](https://nlnetlabs.nl/projects/unbound/about/), [WireGuard](https://www.wireguard.com/), and [AdGuard Home](https://adguard.com/en/adguard-home/overview.html).

---

Remember to replace placeholder values and paths with your actual configuration details. Keep your configuration files secure, especially if they contain sensitive information.