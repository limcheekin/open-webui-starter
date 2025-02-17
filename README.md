# Open Webui Starter
=====================

The Open Webui Starter project is meant to provide a quick template fork 
setting up [Open Webui](https://openwebui.com/). More information can be found 
about configurations on the [Open Webui Docs](https://docs.openwebui.com/).


## Project Overview

The Open Webui Starter project is an open-source web application that 
integrates with various Large Language Models (LLMs) and provides a 
user-friendly interface for interacting with them. This project includes the 
following tooling and applications:

- **Ollama**: Get up and running with large language models.
- **Open Webui**: Open WebUI is an extensible, feature-rich, and user-friendly self-hosted AI platform designed to operate entirely offline
- **Edge TTS**: Python module that allows you to use Microsoft Edge's online text-to-speech service
- **Redis**: An open source-available, in-memory storage, used as a distributed, in-memory key–value database, cache and message broker, with optional durability
- **Postgresql/PgVector**: A free and open-source relational database management system (RDBMS) emphasizing extensibility and SQL compliance (has vector addon)
- **Searxng**: A free internet metasearch engine for open webui tool integration
- **Nginx**: A web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache
- **Certbot**: A tool that helps with setup and configuring of SSL certificates


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

1. Clone this repository using your preferred Git client.

```sh
git clone https://github.com/iamobservable/open-webui-starter.git
```

2. Create a new environment files. Update them with your environment changes.

```sh
cp conf/nginx/challenge.conf.example conf/nginx/challenge.conf
cp conf/nginx/default.conf.example conf/nginx/default.conf

cp data/searxng/settings.yml.example data/searxng/settings.yml
cp data/searxng/uwsgi.ini.example data/searxng/uwsgi.ini
cp conf/nginx/default.conf.example conf/nginx/default.conf

cp env/auth.example env/auth.env
cp env/db.example env/db.env
cp env/edgetts.example env/edgetts.env
cp env/ollama.example env/ollama.env
cp env/openwebui.example env/openwebui.env
cp env/redis.example env/redis.env
cp env/searxng.example env/searxng.env
```

***The environment files can contain sensitive information such as API keys 
and passwords. Do not check them into source control.

3. Add a unique SEARXNG_SEARCH value to your `env/searxng` file

3. Execute docker containers to start your environment

```sh
docker compose up -d
```

4. Once the containers are started, access the Open Webui platform by visiting 
`http://localhost/` in your web browser.


### Optional SSL Installation

We will be using certbot to generate a free SSL certificate. More can be found on using certbot within the [Certbot User Guide](https://eff-certbot.readthedocs.io/en/latest/using.html).

***Automated renewal information can be found at [this link](https://eff-certbot.readthedocs.io/en/latest/using.html#setting-up-automated-renewal).***

1. Configure your custom domain tld to point to the IP of the nginx docker container (or a forwarding IP) - create both A and AAAA records
2. Modify your /conf/nginx/challenge.conf with the custom domain (1 place in the file)
1. Modify the nginx section in your compose.yaml to replace the default.conf with challenge.conf file
3. Create a new ssl certificate using the command below (update the email and domain that should be associated with the certificate)
```sh
docker compose -f compose.yaml run --rm certbot certonly --webroot --webroot-path=/var/www/certbot --email <email> --agree-tos --no-eff-email --force-renewal -d <domain>
```
4. Update your /conf/nginx/default.conf file with the custom domain (4 places in the file)
5. Modify the nginx section in your compose.yaml to replace the challenge.conf with default.conf file

***More information on [configuring SSL with certbot](https://medium.com/@dinusai05/setting-up-a-secure-reverse-proxy-with-https-using-docker-compose-nginx-and-certbot-lets-encrypt-cfd012c53ca0)

## Contribution

Contributions to the Open Webui Starter project are welcome. If you'd like to 
contribute, please fork this repository and submit a pull request with any 
changes or additions.

