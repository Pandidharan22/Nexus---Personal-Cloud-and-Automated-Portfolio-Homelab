# Product Requirements Document (PRD)
**Project Name:** Project Nexus (Personal Cloud & Automated Portfolio Homelab)
**Platform:** Windows OS (Host) via Docker Desktop / WSL 2
**Primary Domain:** `pandidharan.dev`
**Author:** Pandidharan
**Target Audience:** AI Engineers, Cloud Developers, and Tech Professionals.

## 1. Executive Summary
The objective of this project is to architect a production-grade, dual-purpose server on a local Windows machine. Operating silently via Docker containerization, the system will serve two primary functions:
1.  A highly secure, globally accessible private cloud storage system.
2.  A high-performance web server hosting a complex React Three Fiber (R3F) 3D portfolio, featuring a fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline.

The infrastructure leverages Cloudflare Zero Trust to expose services to the internet securely without local port forwarding, ensuring a zero-surface-area attack vector on the local network.

## 2. Architecture & Technology Stack
*   **Host Environment:** Windows OS with WSL 2 integration.
*   **Orchestration:** Docker & Docker Compose.
*   **Reverse Proxy & Networking:** Cloudflare Tunnel (`cloudflared`).
*   **Storage Application:** Nextcloud + MariaDB.
*   **Web Server:** Nginx.
*   **CI/CD Pipeline:** GitHub Actions (Self-Hosted Runner).

## 3. Phased Development Roadmap

### Phase 1: Private Cloud Infrastructure (Storage Server)
**Objective:** Deploy a globally accessible, scalable storage solution that bypasses third-party cloud providers, writing data directly to local physical disks.
*   **Routing:** `cloud.pandidharan.dev` -> Cloudflare Tunnel -> `nextcloud_app:80`
*   **Core Requirements:**
    *   **Containerization:** Deploy Nextcloud and a dedicated MariaDB database on an isolated internal Docker network.
    *   **Data Persistence:** Implement Docker Bind Mounts (`./cloud-data`) to map internal container storage directly to the Windows physical drive.
    *   **Security:** 
        *   Migrate all secrets (Database credentials, Tunnel tokens) to a `.env` file, explicitly ignored in version control (`.gitignore`).
        *   Configure Nextcloud `trusted_domains` to strictly accept traffic only from `cloud.pandidharan.dev`.

### Phase 2: Automated Web Hosting (Portfolio & CI/CD)
**Objective:** Deploy an automated web hosting environment capable of serving a computationally heavy React 3D portfolio, automatically updating whenever new code is pushed to GitHub.
*   **Routing:** `pandidharan.dev` -> Cloudflare Tunnel -> `nginx_portfolio:80`
*   **Core Requirements:**
    *   **Static Serving:** Deploy an Nginx container optimized to serve pre-built React (CSR) static files from a bind-mounted directory (`./portfolio-data`).
    *   **Self-Hosted Runner:** Install and register a GitHub Actions Runner directly on the Windows host machine.
    *   **Automation (main.yml):** Architect a GitHub Actions workflow that triggers on a push to the `main` branch. The workflow must:
        1.  Detect the code change.
        2.  Execute the build sequence (`npm install` and `npm run build`).
        3.  Automatically transfer the generated `dist` files into the Nginx bind mount directory, instantly updating the live site without container downtime.

## 4. Security & Compliance
*   **Zero Local Ports:** No router ports (e.g., 80, 443, 22) will be opened. All inbound traffic is explicitly denied at the router level.
*   **Outbound Tunnels:** The `cloudflared` daemon will establish an outbound-only encrypted connection to the Cloudflare Edge.
*   **Code Separation:** The Homelab infrastructure repository (`docker-compose.yml`, scripts) must remain strictly decoupled from the Portfolio source code repository.

## 5. Success Metrics & Verification

**Phase 1 Verification:**
*   [ ] The command `docker compose up -d` executes successfully with no crash loops.
*   [ ] Navigating to `cloud.pandidharan.dev` on a cellular network (off Wi-Fi) loads the Nextcloud GUI.
*   [ ] Files uploaded via the Nextcloud mobile app physically populate within the host's `cloud-data` directory.

**Phase 2 Verification:**
*   [ ] Navigating to `pandidharan.dev` successfully renders the React Three Fiber 3D environment with no mixed-content or CORS errors.
*   [ ] Pushing a minor text change to the portfolio's GitHub repository automatically triggers the Self-Hosted Runner.
*   [ ] The live website reflects the pushed changes within 60 seconds of the GitHub Action completing, requiring zero manual intervention on the host machine.
