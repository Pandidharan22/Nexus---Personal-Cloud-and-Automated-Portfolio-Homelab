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

### Phase 2: Automated Web Hosting & CI/CD (Completed)
- **Objective:** Host a computationally heavy React 3D portfolio with zero-touch automated deployments.
- **Architecture:** Nginx (Static Serving) + GitHub Actions Self-Hosted Runner.
- **Milestones Achieved:**
  - Bound Nginx to a local directory for blazing-fast static file serving.
  - Registered a Windows-native GitHub Runner directly on the host machine.
  - Engineered a CI/CD pipeline (`deploy.yml`) that triggers on `main` branch pushes.
  - Implemented a "nuke and pave" deployment script to ensure clean rollouts without server bloat.