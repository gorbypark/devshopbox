# devshopbox [WIP]

## About
Devshopbox is a fairly simple docker-compose based project to get you up and running with a self-hosted and web-based development environment.  It includes:

- Traefik (reverse proxy that auto provisions Let's Encrypt SSL certificates and sub-domains
- Gitea (web based git interface)
- Drone (CI/CD)
- Code Server (VisualStudio Code in the browser)

##Requirments
- A linux host with docker-compose and at least 2GB of RAM (1GB or less is fine if not using code-server)
- A domain name

## How to
- Add a wildcard A record that points to your linux host/VPS (ie:  *.mydomain.com -> ip.of.linux.host)
- Open the file at `./traefik/traefik.conf` and configure for your domain
- Add a file named .env into the root of the project folder with your configuration variables.
- Run `docker-compose up -d` in the root folder of the project
- You should now have three subdomains, all protected with SSL certificates pointing to the three apps (gitea, drone, code-server).
- Visit the gitea subdomain to configure it.  Please use the SQLite database as this project doesn't provision a PostgreSQL or MySQL database for you.  In the Gitea setup wizard, change the base url to `https://<subdomainfromenvfile>.mydomain.com`

## Warning
This is a work in progress.  Although I have kept security in mind, there's no guarantee of any.  Also, data is kept in docker volumes in the root dir of the project, so keep those backed up if you care about your data.  As above, there's no guarantee about your data.
