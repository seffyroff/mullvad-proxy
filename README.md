# FORK NOTICE

This fork removes the locally built mullvad client container, as in my experience, it doesn't work in Docker.  I've replaced this with linuxserver/wireguard, which needs a wg0.conf file from your Mullvad account config page placed in ./config/.

# Mullvad VPN HTTP and SOCKS5 Proxy using Docker

This docker compose setup runs the [Mullvad VPN app](https://mullvad.net/en/download/linux/) inside a docker container and exposes an HTTP proxy using privoxy (port 8118) and SOCKS5 proxy using nginx (port 1080). The Mullvad CLI can still be used to control the application.

## Why

Mullvad VPN app works well and has easy to use CLI functionality for switching between their worldwide servers. However, when it is enabled, it will route all of your internet traffic through it and sometimes that is not ideal. If you want have VPN access for individual applications (e.g. Firefox but not Chrome) without routing all your internet traffic through it, then this setup will work with any applications that can use a HTTP or a SOCKS5 proxy server.

## Requirements and Usage

This setup has been tested on Ubuntu Linux and should work with all Debian-based Linux OSes. You will need the following to use this repository:
- git
- [Docker](https://docs.docker.com/engine/install/ubuntu/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- An active [Mullvad VPN](https://mullvad.net/en/) Account Number

Once you have an active account, go to the [Mullvad Manage ports and WireGuard keys page](https://mullvad.net/en/account/#/ports) and make sure you have at least 1 wireguard key available to allow the Mullvad VPN container to connect. Each account number has a total of 5 wireguard keys.

Make sure ports 8118 and 1080 are available. If you need to bind the proxy servers to different ports, you can create a `.env` in the same path as the docker-compose.yml file with the following to change the ports:

```bash
HTTP_PORT=8118
SOCKS5_PORT=1080
```

Once the above is setup, run the following to get the proxy servers up and running:

```bash
git clone https://github.com/seffyroff/mullvad-proxy.git
cd mullvad-proxy
docker-compose up
```

The setup.sh script runs `docker-compose up -d` to setup Mullvad VPN and proxy docker  containers with container networking and then runs several Mullvad VPN CLI commands to setup the app with the account number and connects to the VPN. The containers are configured to auto restart and will be available whenever your machine starts as long as docker is running.