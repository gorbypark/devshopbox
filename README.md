# devshopbox [WIP]

## About

Devshopbox is a fairly simple docker-compose based project to get you up and running with a self-hosted and web-based development environment. It includes:

- Traefik (reverse proxy that auto provisions Let's Encrypt SSL certificates and sub-domains)
- Gitea (web based git interface)
- Drone (CI/CD)
- Code Server (VisualStudio Code in the browser)

## Requirements

- A linux host with docker-compose and at least 2GB of RAM (1GB or less is fine if not using code-server)
- A domain name and control over it's DNS records

## How To

- Add a wildcard A record that points to your linux host/VPS (ie: \*.mydomain.com -> ip.of.linux.host)
- Open the file at `./traefik/traefik.toml` and configure for your domain
- Add a file named .env into the root of the project folder with your configuration variables as seen below.

```
ROOT_DOMAIN=mydomain.com
TRAEFIK_VERSION=latest
GITEA_SUB_DOMAIN=git
GITEA_VERSION=latest
DRONE_SUB_DOMAIN=ci
DRONE_VERSION=latest
VSCODE_SUB_DOMAIN=vscode
VSCODE_PASSWORD=sup3rs3cr3t
VSCODE_VERSION=latest
```

- Run `docker-compose up -d` in the root folder of the project
- You should now have three subdomains, all protected with SSL certificates pointing to the three apps (gitea, drone, code-server).
- Visit the gitea subdomain to configure it. Please use the SQLite database as this project doesn't provision a PostgreSQL or MySQL database for you. In the Gitea setup wizard, change the base url to `https://<subdomainfromenvfile>.mydomain.com`

## Security Warning

This is a work in progress. Although I have kept security in mind, there's no guarantee of any. Also, data is kept in docker volumes in the root dir of the project, so keep those backed up if you care about your data. As above, there's no guarantee about your data. Both traefik and drone are configured to have direct access to the hosts `/var/run/docker.sock`. While they are both configured as per the instructions for each project, it is debatable as to whether this could pose a security concern or not. If either the traefik or drone containers were breeched, attackers will have full control over the hosts docker daemon, which pretty much means they would have full control over the host itself.
