# OpenVPN Toolkit

This directory contains the tools to set up a new OpenVPN server including the tools provided by `easy-rsa` to create certificates and keys. This guide follows and builds on the instructions provided [here](https://openvpn.net/community-resources/setting-up-your-own-certificate-authority-ca/).

## Dependencies

Make sure you have `openvpn` and `easy-rsa` installed on your PC: `sudo apt install openvpn easy-rsa`

## Setup

1. If they do not exist, create `conf/` and `conf/ccd` empty directories.
2. Verify that your `/etc/openvpn/` directory constains a directory called `client`. If it doesn't exist, create it.
3. Navigate to the `ca/` directory and edit the `vars` file with your config.
4. Run the `clean-all` script.

### Creating a certificate authority

1. Navigate to the `ca/` directory and source the `vars` file.
2. Run the `build-ca` scipt and follow prompts: `./build-ca`

### Create a server certificate and key

1. Navigate to the `ca/` directory and source the `vars` file.
2. Run the `build-key-server` scripts and follow prompts: `./build-key-server <SERVER_NAME>`

### Create client keys

1. Navigate to the `ca/` directory and source the `vars` file.
2. Run the `build-key` scripts and follow prompts: `./build-key <KEY_NAME>`
3. If you haven't already created a dh parameter file, built it now: `./build-dh`

### Create server and client conf files

For more details on what can go in the conf files, look at `sample-client.conf` and `sample-server.conf` in the `samples/` directory.

**Client config:** `build-client-conf` 
Args:
- KEY_NAME
- SERVER_ADDRESS
- PORT
- PROTOCOL

Example: `./build-client-conf client1 192.168.1.3 1194 tcp`

**Server config:** `build-server-conf`
Args:
- SERVER_NAME
- SUBNET
- PORT
- PROTOCOL

Example: `./build-server-conf server 10.8.0.0 1194 tcp`

**Set static IP for clients**: `add-ccd`
Args:
- CLIENT
- ADDRESS

Example: `./add-ccd client1 10.8.0.10`

## Usage

To test the server or client, navigate to the `conf/` directory and run: `sudo openvpn <SERVER_NAME>.conf`

**Deploy VPN server**: `deploy-server` copies required files to the main openvpn directory.
Args:
- SERVER

Example: `./deploy-server server`
- Start server: `sudo systemctl start openvpn-server@<SERVER_NAME>`
- Stop server: `sudo systemctl stop openvpn-server@<SERVER_NAME>`

**Deploy VPN client**: `deploy-client` copies client config to main openvpn directory.
Args:
- CLIENT

Example: `./deploy-client client1`
- Start client: `sudo systemctl start openvpn-client@<CLIENT_NAME>`
- Stop client: `sudo systemctl stop openvpn-client@<CLIENT_NAME>`

## Other tips

- To disable an existing key, run `revoke-full`: `./revoke-full <KEY_NAME>`
- To clear out your entire setup (ceritifcates, keys, etc.), run `reset-ca`
