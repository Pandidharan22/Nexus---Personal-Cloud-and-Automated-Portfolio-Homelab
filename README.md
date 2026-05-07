# Project Nexus: Personal Cloud & Automated Portfolio Homelab 🌐

A production-grade, dual-purpose server architected on a local Windows machine. Operating silently via Docker containerization, Project Nexus serves as a highly secure, globally accessible private cloud and a high-performance web server hosting a complex React Three Fiber (R3F) 3D portfolio.

Built with a **Zero-Surface-Area** security philosophy, the infrastructure leverages Cloudflare Zero Trust to expose services to the internet securely without relying on local port forwarding.

## 🚀 Core Architecture

Project Nexus is divided into two highly optimized, isolated systems running within a unified Docker network:

### 1. Private Cloud Infrastructure (Data Sovereignty)
- **Stack:** Nextcloud + MariaDB
- **Function:** A globally accessible storage server that bypasses third-party cloud providers. Data is written directly to local physical disks via Docker bind mounts, ensuring absolute data ownership.
- **Routing:** `cloud.pandidharan.dev` ➡️ Cloudflare Tunnel ➡️ `nextcloud_app:80`

### 2. Automated Web Hosting & CI/CD Pipeline
- **Stack:** Nginx + GitHub Actions (Self-Hosted Runner)
- **Function:** An Nginx web server optimized to serve pre-built React static files. A GitHub Actions Runner operates directly on the host machine, automatically fetching, building, and deploying code pushes from the portfolio repository directly to the live server with zero downtime.
- **Routing:** `pandidharan.dev` ➡️ Cloudflare Tunnel ➡️ `nginx_portfolio:80`

## 🛠️ Technology Stack

* **Host Environment:** Windows OS with WSL 2 integration
* **Containerization:** Docker & Docker Compose
* **Reverse Proxy & Ingress:** Cloudflare Tunnel (`cloudflared`)
* **Web Server:** Nginx (Alpine)
* **Cloud Application:** Nextcloud
* **Database:** MariaDB
* **CI/CD:** GitHub Actions (Windows Self-Hosted Runner)

## 🔒 Security & Compliance

* **Zero Local Ports:** No router ports (80, 443, 22) are open. Inbound traffic is explicitly denied at the router level.
* **Outbound Tunnels:** The `cloudflared` daemon establishes an outbound-only encrypted connection to the Cloudflare Edge.
* **Strict Code Separation:** Homelab infrastructure configuration is decoupled from the Portfolio source code to prevent repository bloat and maintain isolated security contexts.

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