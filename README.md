# Open Webui Starter
=====================

The Open Webui Starter project is meant to provide a quick template fork 
setting up [Open Webui](https://openwebui.com/). More information can be found 
about configurations on the [Open Webui Docs](https://docs.openwebui.com/).


## Project Overview

The Open Webui Starter project is a entry into using the open-source project 
Open Webui. It integrates with various Large Language Models (LLMs) and provides 
a private, user-friendly, and local interface for interacting with "AI". This 
starter project includes the following tooling and applications:

- **[Ollama](https://ollama.com/)**: Get up and running with large language models.
- **[Open Webui](https://openwebui.com/)**: Open WebUI is an extensible, feature-rich, and user-friendly self-hosted AI platform designed to operate entirely offline
- **[Edge TTS](https://github.com/rany2/edge-tts)**: Python module that allows you to use Microsoft Edge's online text-to-speech service
- **[Redis](https://redis.io/)**: An open source-available, in-memory storage, used as a distributed, in-memory key–value database, cache and message broker, with optional durability
- **[Postgresql](https://www.postgresql.org/)/[PgVector](https://github.com/pgvector/pgvector)**: A free and open-source relational database management system (RDBMS) emphasizing extensibility and SQL compliance (has vector addon)
- **[Searxng](https://docs.searxng.org/)**: A free internet metasearch engine for open webui tool integration
- **[Nginx](https://nginx.org/)**: A web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache
- **[Cloudflare](https://www.cloudflare.com/)**: Cloudflare hosting and tunnel tool providing anonymous proxying and SSL certificates
- **[Watchtower](https://github.com/containrrr/watchtower)**: A process for automating Docker container base image updates.


## Table of Contents
1. [Dependencies](#dependencies)
2. [Installation](#installation)
3. [Contribution](#contribution)


## Dependencies

The following dependencies are required to run the Open Webui Starter project:

- **Git**: Version control system for managing code changes
- **Docker**: Containerization platform for running and deploying applications


## Installation

To install the Open Webui Starter project, follow these steps:

1. Clone this repository using your preferred Git client

```sh
git clone https://github.com/iamobservable/open-webui-starter.git
```

2. Create new environment files and update with your environment changes

```sh
cp conf/cloudflared/config.example conf/cloudflared/config.yml
cp conf/nginx/default.example conf/nginx/default.conf

cp data/searxng/settings.yml.example data/searxng/settings.yml
cp data/searxng/uwsgi.ini.example data/searxng/uwsgi.ini

cp env/auth.example env/auth.env
cp env/cloudflared.example env/cloudflared.env
cp env/db.example env/db.env
cp env/edgetts.example env/edgetts.env
cp env/ollama.example env/ollama.env
cp env/openwebui.example env/openwebui.env
cp env/redis.example env/redis.env
cp env/searxng.example env/searxng.env
```

*The environment files can contain sensitive information such as API keys 
and passwords. Do not check them into source control.*

3. Add a unique SEARXNG_SECRET value to your [env/searxng.env](http://github.com/iamobservable/open-webui-starter/blob/main/env/searxng.example#L3)
4. Add a unique WEBUI_SECRET_KEY to both your [env/auth.env](http://github.com/iamobservable/open-webui-starter/blob/main/env/auth.example#L2) and 
[env/openwebui.env](http://github.com/iamobservable/open-webui-starter/blob/main/env/openwebui.example#L10); MAKE SURE THEY MATCH. This allows jwt token authentication to work with the main Open Webui (/), swagger (/docs), redis (/redis), and searxng (/searxng)
5. Create a Cloudflare account and configure a Zero Trust Network tunnel.
6. Add or create your domain, so Cloudflare can manage it.
7. Create a new tunnel, this will allow traffic to flow to your local machine.
8. Set the tunnel id in your [conf/cloudflared/config.yml](https://github.com/iamobservable/open-webui-starter/blob/main/conf/cloudflare/config.example#L1)
9. Add tunnel token within your [env/cloudflared.env](http://github.com/iamobservable/open-webui-starter/blob/main/env/cloudflared.example#L1)
10. Update your [conf/nginx/default.conf](https://github.com/iamobservable/open-webui-starter/blob/main/conf/nginx/default.example#L34) with your domain
11. Execute docker containers to start your environment

```sh
docker compose up -d
```

Once the containers are started, access the Open Webui platform by visiting 
`http://<domain-name>/` in your web browser.



## Migrating from Sqlite to Postgresql

*** Tested using node v22.12.0 ***

Depending on your setup, you may need to expose your postgresql (db) container 
port before proceeding. By default, this project's db container is not exposing 
a port. 

***example***

```yaml
services:
  db:
image: pgvector/pgvector:pg15
    ports:
      - 5432:5432
```

1. Change to the migrator folder and install node packages

```bash
cd migrator
npm install
```

2. Execute migrate.js file with node. Adjust the following command with your 
specific configuration locations, user, password, and database names.

```bash
node migrate.js ../data/openwebui/webui.db "postgresql://postgres:postgres@localhost/openwebui"
```

3. Remove and restart your postgresql container
```bash
docker compose down db && docker compose up db -d
```

## Watchtower and Notifications

A Watchtower container provides a convenient way to check in on your container 
versions to see if updates have been released. Once updates are found, Watchtower 
will pull the latest container image(s), stop the currently running container and 
start a new container based on the new image. After completing its process, 
Watchtower can send notifications to you. More can be found on notifications via 
the [Watchtower website](https://containrrr.dev/watchtower/notifications/).

For the sake of simplicity, this document will cover the instructions for setting 
up notifications via Discord. If you desire to be more detailed in your configuration, 
the [arguments section](https://containrrr.dev/watchtower/arguments/) describes 
additional settings available for the watchtower setup.

1. Edit your [env/watchtower.env](https://github.com/iamobservable/open-webui-starter/blob/main/env/watchtower.example#L2) with your discord link. [More information](https://containrrr.dev/shoutrrr/v0.8/services/discord/) is provided on how to create your discord link (token@webhookid).
2. Restart your watchtower container

```bash
docker compose down watchtower && docker compose up watchtower -d
```


## Contribution

Contributions to the Open Webui Starter project are welcome. If you'd like to 
contribute, please fork this repository and submit a pull request with any 
changes or additions.

