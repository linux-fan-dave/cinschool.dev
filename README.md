# Cin School Dev

All services started from a single repository

_Keep in mind this repository is for local development only and is not meant to be deployed on any production environment!_

## Requirements

1. [Docker](https://docs.docker.com/install/)
2. [Docker Compose](https://docs.docker.com/compose/install/)

## How to run it?

1. Clone the repository:

```
git clone https://github.com/Concept-in-Code/cinschool.dev/ --recursive cinchool.dev
```

2. Shared folders:
  We are using shared folders to enable live code reloading. Without this, Docker Compose will not start:
    - Windows/MacOS: Add the cloned `cinchool.dev` directory to Docker shared directories (Preferences -> Resources -> File sharing). 
    - Linux: No action required, sharing already enabled and memory for Docker engine is not limited.

3. Go to the cloned directory:

```
cd cinchool.dev
```

4. Build the stack:

```
docker compose build --no-cache
```

5. Run the application:

```
docker compose up
```

In case of server development you should run the server within the IDE and not within docker compose. In that case you can use the scale flag, e.g.:

```
docker compose up --scale server=0
```

## Tools and URLs

After building and running the application following URLs are exposed:

- Portal (public area): http://localhost:7010
- Admin (restricted area): http://localhost:7011
- Server: http://localhost:7012
- Database: http://localhost:7013

## Specification and data model

The folder `specs` contains the datamodel and is a [Modelio](https://github.com/ModelioOpenSource/Modelio) project. To open the project open Modelio and switch the workspace to `specs`. 

in `specs/exports` are the exported diagrams as images.

## How to update the subprojects to the newest versions?

This repository contains newest stable versions.
When new release appear, pull new version of this repository.
In order to update all of them to their newest versions, run:

```
git submodule update --remote
```

During the development there might be changes in existing changelog files. Therefore the whole database need to be purged. To setup a clean database you have to purge the volume before starting the stack:

```
docker compose down -v
```