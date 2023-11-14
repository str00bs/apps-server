# Home (development) Server
<img src="https://images.unsplash.com/photo-1546124404-9e7e3cac2ec1?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1" width="600">
<!-- Thanks to unsplash and https://unsplash.com/@isodme for royalty free stock photos! -->


Open-source fueled apps, services and tools for a self hosting & development.


## Overview
The templates are grouped into three types:
- `core`: Required core services to deploy & manage other services
- `services`: Single service templates
- `stacks`: Multi-service templates

**Core**
- `gateway` | `traefik`: Container native proxy & service gateway
- `admin` | `portainer`: Container native monitoring and administration interface

**Services**:
- `vault` | `vaultwarden`: Open-source rust implementation of the password manager bitwarden
- `storage` | `minio`: Open-source object storage
- `automations` | `n8n`: Open-source integration & flow based automation tool
- `registry`: Docker container registry
- `ctc`: Container Test Case to confirm deployment & letsencrypt certificate generation
  
**Stacks**:
- `ci` | `drone`: Open-source CI/CD pipeline and deployment powered by community integrations
- `cloud` | `nextcloud`: Open-source GCloud/Dropbox alternative
- `writeups` | `ghost`: Open-source publication tool and wordpress alternative
- `analytics` | `matomo`: Open-source GDPR compliant analytics platform

**W.I.P.**
- `deploy`: A generalized way to auto-deploy new verified updates (Either via script alone or together with TerraForm)
- `backups`: A generalized way to backup and manage the system
- `logs & metrics`: A generalized way to view and manage logs (Prometheus)
- `security`: A generalized way to scan the deployed VLANs and services for malicious activity. (TemplarBit?)


## Setup
<img src="https://images.unsplash.com/photo-1595776613215-fe04b78de7d0?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" width="600">
<!-- Thanks to unsplash and https://unsplash.com/@walling for royalty free stock photos! -->


### Requirements
- Note all `DOMAIN` variables are **without HTTP(S) prefix!**
    - Valid domain: `vault.hs.example.com`
    - Invalid doman: `https://vault.hs.example.com`

- DNS `A` record with a wildcard `sub.domain.tld` pointing towards the server
- Domain & global storage `.env` variables are **required** for the gateway and letsencrypt to work
- A server with at least 2 core CPU and 4GB of RAM
    - Note: Start-up may require more RAM or slow-down with only 2 cores/CPUs
    - Note: Varies depending on load, usage and number of services currently running
    - Note: Once deployed, all current services requires ~2.1GB of RAM


### Configuration & Deployment
The gateway detects and auto-adds new services as they're deployed so it should be deployed first with either option!

**Automatic deployment**
- New and improved auto-deploy to be added in the future =)
<!-- 1. Read `deploy.py` to see what it does
2. Setup with `python3 deploy.py`
3. Deploy with `python3 deploy.py --ARG`, args=["core", "services", "stacks", "all"] -->

**Manual deployment**
1. Copy `dist.env` and `dist.docker-compose.yml` files for core with your preferred configuration
2. Pull and start core services with `docker-compose up` from the service directory and verify that they're running (with letsencrypt provided SSL certs)
3. Copy `dist.env` and `dist.docker-compose.yml` files for your preferred service/stack setup
4. Pull and start preferred services with `docker-compose up -d` from the serviec directory.
