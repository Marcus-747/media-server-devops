![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Ansible](https://img.shields.io/badge/Ansible-2.15+-EE0000?logo=ansible&logoColor=white)
![Docker](https://img.shields.io/badge/Docker_Compose-v2+-2496ED?logo=docker&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?logo=prometheus&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-2088FF?logo=githubactions&logoColor=white)

---

```mermaid
flowchart TD
    subgraph Developer Workspace
        DEV[Developer / Git Client]
    end

    subgraph GitHub
        GHA[GitHub Actions CI/CD]
    end

    subgraph Production Host [Ubuntu Linux Server]
        subgraph Security Layer
            UFW[UFW Firewall / Fail2ban]
            NPM[Nginx Proxy Manager / SSL]
        end

        subgraph Core Services [Docker Network: media-net]
            Jellyfin[Jellyfin Media Server]
            Radarr[Radarr - Movies]
            Sonarr[Sonarr - TV Shows]
            qBit[qBittorrent - Downloader]
        end

        subgraph Observability Layer [Docker Network: monitor-net]
            NodeExp[Node Exporter]
            Prom[Prometheus]
            Grafana[Grafana Dashboards]
        end
    end

    DEV -->|git push| GHA
    GHA -->|SSH / Docker Compose Deploy| Production Host
    
    Users[Пользователи / Smart TV / Mobile] -->|HTTPS| NPM
    NPM --> Jellyfin
    
    Prom --> NodeExp
    Prom --> Jellyfin
    Grafana -->|Queries| Prom
