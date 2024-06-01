# Cin School Dev

All services started from a single repository

## Table of contents

- [Cin School Dev](#cin-school-dev)
  - [Table of contents](#table-of-contents)
  - [Requirements](#requirements)
  - [Folders](#folders)
  - [How to run it?](#how-to-run-it)
  - [Windows Development](#windows-development)
    - [VPN \& Proxy support for WSL2](#vpn--proxy-support-for-wsl2)
    - [Specialities of Windows Development](#specialities-of-windows-development)
  - [Server](#server)
  - [Tools and URLs](#tools-and-urls)
    - [Swagger](#swagger)
  - [How to update the subprojects to the newest versions?](#how-to-update-the-subprojects-to-the-newest-versions)


## Requirements

1. [Docker](https://docs.docker.com/install/)
2. [Docker Compose](https://docs.docker.com/compose/install/)

## Folders

**slient**:
Git submodule containing the client application

**server**:
Git submodule containing the server application

**docs:**
Exports of all session presentations

## How to run it?

1. Clone the repository:

```
git clone https://github.com/Concept-in-Code/cinschool.dev/ --recursive cinschool.dev
```

2. Shared folders:
  We are using shared folders to enable live code reloading. Without this, Docker Compose will not start:
    - Windows/MacOS: Add the cloned `cinchool.dev` directory to Docker shared directories (Preferences -> Resources -> File sharing). 
    - Linux: No action required, sharing already enabled and memory for Docker engine is not limited.

3. Go to the cloned directory:

```
cd cinschool.dev
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

## Windows Development

There are several problems with docker (compose) on windows especially with shared folders. According to the [docker on windows doc](https://docs.docker.com/desktop/wsl/) WSL2 should be used for filesystem sharing. Please follow the steps closely and make sure that you have a WSL2 linux os (e.g. ubuntu) up and running with docker integration enabled.
Most prominent steps include:

* install WSL2
* enable WSL2 as default host system
* convert existing wsl system to WSL2
* install [docker desktop for windows](https://docs.docker.com/desktop/install/windows-install/)

### VPN & Proxy support for WSL2
In a company environment the user might be required to use a VPN service / proxy. A helper tool, [wsl-vpnkit](https://github.com/sakai135/wsl-vpnkit) exists that helps with this problems. First download the package then install using the following command in a elevated powershell:

```
wsl --import wsl-vpnkit --version 2 $env:USERPROFILE\wsl-vpnkit wsl-vpnkit.tar.gz
```

In order to execute the best is to create a powershell script to make it easy to execute in a privileged environment. Create a powershell script (I use the name: wsl2-proxy.ps1) and execute this script elevated.

```
wsl.exe -d wsl-vpnkit --cd /app wsl-vpnkit
```

### Specialities of Windows Development
In order to use shared folders and INotify support the code should be cloned on the WSL / Linux environment. However the code should / could be edited in the visual studio code environment from windows. In order to use wsl from the windows environment the [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension must be installed in the windows visual studio code. After that the following steps should be executed in a wsl shell (open via start menu -> wsl)

```
git clone https://github.com/Concept-in-Code/cinschool.dev/ --recursive cinschool.dev
cd cinschool.dev
code .
```

The commands to start the docker environment must be executed from the WSL shell again

```
cd cinschool.dev
docker compuse up
```

## Server

For the server we use a [.Net](https://github.com/Concept-in-Code/realworlddotnet) implementation of the [RealWorld](https://main--realworld-docs.netlify.app/) specification.   


## Tools and URLs

After building and running the application following URLs are exposed:

- Portal (public area): http://localhost:7010
- Admin (restricted area): http://localhost:7011
- Server: http://localhost:7012
- Database: http://localhost:7013

### Swagger

The Swagger Documentation can be found here:

http://localhost:7012/swagger

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