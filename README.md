# Nexus - Personal Cloud and Automated Portfolio Homelab

## Development Journal

### Phase 1: Private Cloud Infrastructure (Completed)
- **Objective:** Deploy a secure, self-hosted cloud storage solution.
- **Architecture:** Nextcloud + MariaDB + Cloudflare Tunnels via Docker Compose.
- **Milestones Achieved:**
  - Configured isolated Docker networks for zero-surface-area local security.
  - Implemented Docker Bind Mounts (`./cloud-data`) ensuring physical data sovereignty on the host machine.
  - Successfully bypassed router port-forwarding using a reverse-proxy Cloudflare Zero Trust Tunnel.
  - Resolved proxy loop redirect errors using `occ` CLI configurations.