# Navigate deploy

Navigate was built using multiple services, currently three docker containers are needed to start the entire project, these containers are

- server: It is the scala backend in charge of the communication with the real devices, also starts a webserver to serve the ui files and a proxy to route the client requests to the configs container.
- configs: Is the graphql API developed in typescript using Apollo to connect the ui and the database container, the server works as a proxy to route the ui requests.
- database: The Postgres database to host multiple configurations required by the UI and display the current used configuration and targets.

The following diagram represents the current architecture
![architecture](docs/img/architecture.svg)

## Compose

Server and configs containers are started from custom docker images which are hosted in dockerhub using the noirlab software account **nlsoftware**. The database container is started from the default postgres image also hosted in dockerhub.

This repository includes a template file `template-env` for the environment variables that will be used by the docker compose command, if you do not want to explicitely provide the environment file, **template-env** should be copied to **.env** and modified to match your needs.

If the previous files are already configured the system can be started using the following command (Explicitely using the template-env)

```bash
docker compose -f docker-compose.yml --env-file template-env up -d
```

# Local development configuration

The system can be started locally for development purposes, I highly recommend to use this docker-compose.yml file to start the database container only `docker compose -f docker-compose --env-file template-env up -d database`, otherwise a local installation of postgres database is required.

## [Configs repo](https://github.com/gemini-hlsw/navigate-configs#readme)

### Requirements

- Node
- Environemnt file (.env) with **DATABASE_URL** and **SERVER_PORT** variables

### Running

In the navigate-configs repository run the following commands

```bash
# Install dependencies
pnpm install
# Build project
pnpm build
# Generate database schema
pnpm prisma migrate dev
# Run the server
pnpm preview
```

## [Server]((https://github.com/gemini-hlsw/navigate-server#readme))

### Requirements

- SBT
- JAVA 17 or greather

### Running

Commands

```bash
# Start webserver
sbt navigate_web_server/reStart
```

## [UI]((https://github.com/gemini-hlsw/navigate-ui#readme))

### Requirements

- Node

### Running

Commands:

```bash
# Install dependencies
pnpm install
# Run webserver
pnpm dev
```
