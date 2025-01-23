# Apps Templates
Open-source fueled apps, services and tools for a self hosting & development.


## Available templates
- `admin` | `portainer`: Container native monitoring and administration interface
- `analytics` | `matomo`: Open-source GDPR compliant analytics platform
- `automations` | `n8n`: Open-source integration & flow based automation tool
- `baas` | `appwrite`: Open-source Backend As A Service
- `block_storage` | `minio`: Open-source object storage
- `blog` | `ghost`: Open-source publication tool and wordpress alternative
- `ci` | `drone`: Open-source CI/CD pipeline and deployment powered by community integrations
- `cloud` | `nextcloud`: Open-source GCloud/Dropbox alternative
- `code` | `VSC Web`: Visual Studio Code web client
- `gateway` | `traefik`: Container native proxy & service gateway
- `registry` | `registry`: Docker container registry
- `uptime` | `kuma`: UptimeKuma service monitor.
- `vault` | `vaultwarden`: Open-source rust implementation of the password manager bitwarden


## Pre-requisties
- Note all `DOMAIN` variables are **without HTTP(S) prefix!**
    - Valid domain: `vault.example.com`
    - Invalid doman: `https://vault.example.com`

- DNS `A` record with a wildcard `sub.domain.tld` pointing towards the server
- Domain & global storage `.env` variables are **required** for the gateway and letsencrypt to work
- A server with at least 2 core CPU and 4GB of RAM
    - Note: Start-up may require more RAM or slow-down with only 2 cores/CPUs
    - Note: Varies depending on load, usage and number of services currently running
    - Note: Once deployed, all current services requires ~2.1GB of RAM


## Setup
The [Gateway](stacks/gateway/) Needs to be deployed first to allow LetsEncrypt to provide SSL.
1. Fill in, and copy over the environment files `cp stack.env .env` 
2. Fill in, and copy over the docker files `stack.docker-compose.yfilesml docker-compose.yml`
3. Start the gateway: `docker-compose up -d`

It's highly recommended to deploy [Admin](stacks/admin) next and use this for preceeding stack deployments.

## Deployments
There are two ways to deploy the configured stacks;
1. Using Portainers WebGUI & stacks feature
2. Manually by following the same steps as in setup per stack (per compose file)

## Adding apps
To add a new app container, the following needs to be included:
- `stack.docker-compose.yml` with the following properties:
  - Per service, define traefik labels for exposure
    ```yaml
    labels:
      - traefik.enable=true
      - traefik.http.routers.{SERVICE_NAME}.tls=true
      - traefik.http.routers.{SERVICE_NAME}.tls.certresolver=letsencrypt
      - traefik.http.routers.{SERVICE_NAME}.entrypoints=https
      - traefik.http.routers.{SERVICE_NAME}.rule=Host(`${DOMAIN}`)
      # Services
      - traefik.http.services.{SERVICE_NAME}.loadbalancer.server.port=80
    ```
- `stack.env` with the follwing properties:
  - This is required for domain and storage configuration 
    ```ini
    # Project variables
    COMPOSE_PROJECT_NAME=stack-{STACK_NAME}
    GLOBAL_STORAGE={PATH_TO_STORAGE}
    DOMAIN={YOUR_DOMAIN_NAME}
    # Service variables
    MY_VAR="1234"
    ```
