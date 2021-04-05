# OpenVPN Toolkit

This package uses OpenVPN and EasyRSA 3 for setting up and managing a CA and and VPN server.

## Pre-requisites

**Install OpenVPN:** `sudo apt update && sudo apt install openvpn`

**Install EasRSA 3 (Optional)**

*NOTE: This is not necessary since EasyRSA is already built into this repo. Follow these steps if you want to have a separate EasyRSA copy on your PC for other projects or if you want to use another version.*

1. Download the latest version of EasyRSA [here](https://github.com/OpenVPN/easy-rsa/releases) and extract contents.
2. Rename the extracted folder to `easyrsa3` and move it to `/etc/easyrsa/`.

## Setup

### CA Setup

*This method assumes you are building the CA and keys on the same machine (less secure). If you choose to separate the CA from the key genertaion, follow the instructions given [here](https://community.openvpn.net/openvpn/wiki/EasyRSA3-OpenVPN-Howto) by OpenVPN.*

1. Navigate to the `ca/` directory
2. Edit the `vars` file with your own information and config
3. Initialize the PKI tool: `./easyrsa init-pki`
4. Build a CA: `./easyrsa build-ca nopass`
5. Build a server: `./easyrsa build-server-full SERVER_NAME nopass`
6. Build a client: `./easyrsa build-client-full CLIENT_NAME nopass`
7. Generate Diffie-Hellman key: `./easyrsa gen-dh`

### OpenVPN Setup

*If there are no `conf` or `conf/ccd` directories, create them.*

1. Navigate to the `openvpn` directory
2. Edit the `vars` file with your config
3. Build a server config file: `./build-server-conf SERVER_NAME`
4. Build a client config file: `./build-client-conf CLIENT_NAME`

## Deploy

### Server

Copies required config files to the `OVPN_DEPLOY_PATH` directory defined in the `vars` file.

```bash
$ ./deploy-server NAME
```

Args:
- NAME: Name of the server

### Client 

Copy your client config file to the `/etc/openvpn` directory on your client machine.

## Run

Both server and client have the same command:
```bash
$ sudo openvpn NAME.conf
```

You can also manage the server/client as a service:
```bash
$ sudo service openvpn@NAME start
$ sudo service openvpn@NAME stop
```