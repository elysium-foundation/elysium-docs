---
title: Run Dev Node
sidebar_position: 1
---

# Run a Node on Elysium

Running a full node on a Elysium network allows you to connect to the network, sync with a bootnode, obtain local access
to RPC endpoints, author blocks on the chain, and more.

There are two types of deployments of Elysium, including the Atlantis (Testnet).

## Requirements

The minimum specs recommended to run a node are shown in the following table. For our MainNet
deployments, disk requirements will be higher as the network grows.

| Component | Requirement                                                                              |
|-----------|------------------------------------------------------------------------------------------|
| CPU       | 8 Cores (Fastest per core speed)                                                         | 
| RAM       | 16 GB                                                                                    |
| SSD       | 1 TB (recommended)                                                                       |
| Firewall  | P2P port must be open to incoming traffic |

## Running Ports

The only ports that need to be open for incoming traffic are those designated for P2P. Validator must not have RPC or WS
ports opened.

### Default Ports for a chain Full-Node

| Description | Port        |
|-------------|-------------|
| P2P         | 30333 (TCP) | 
| RPC         | 9933        |
| WS          | 9944        |
| Prometheus  | 9616        |

## Installation with Docker

Elysium node can be spun up quickly using Docker. For more information on installing Docker, please visit this page.
At the time of writing, the Docker version used was 24.0.1. When connecting to Elysium, it will take a few hours to
completely sync the chain data. Make sure that your system meets the requirements.

Create a local directory to store the chain data:

| Network            | Command                             |
|--------------------|-------------------------------------|
| Elysium            | mkdir /var/lib/elysium-data         | 
| Atlantis (Testnet) | mkdir /var/lib/elysium-testnet-data |

Next, make sure you set the ownership and permissions accordingly for the local directory that stores the chain data. In
this case, set the necessary permissions either for a specific or current user (replace DOCKER_USER for the actual user
that will run the docker command):

### Elysium

```sh
# chown to a specific user
chown DOCKER_USER /var/lib/elysium-data

# chown to current user
sudo chown -R $(id -u):$(id -g) /var/lib/elysium-data
```

### Atlantis (Testnet)

```sh
# chown to a specific user
chown DOCKER_USER /var/lib/elysium-testnet-data

# chown to current user
sudo chown -R $(id -u):$(id -g) /var/lib/elysium-testnet-data
```

Now, execute the docker run command. If you are setting up a validator node, make sure to follow the code snippets for
Validator.

### Dev Node Setup
Create a file name as docker-compose.yml and add below code:
```docker compose file
version: '3'

services:
  elysium-dev-node:
    container_name: elysium-dev-node
    image: vaival/elysium-testnet:latest
    ports:
      - 30333:30333 # p2p port
      - 9933:9933 # rpc port
      - 9944:9933 # redirect all traffic of ws to rpc port
      - 9615:9615 # promethus port
    volumes:
      - ./elysium-testnet-data:/data
    command: [
      "--name", "elysium-dev-node",
      "--dev",
      "--rpc-external",
      "--rpc-cors", "all"
    ]
```
After saving the file run the below code:
```
docker compose up
```
Docker engine will start the node, you can connect it locally by using below urls:

### Local RPC Access:
```
https://127.0.0.1:9933
```
### Local Websocket Access:
```
ws://127.0.0.1:9944
```

Polkadot provides a UI to connect and view your local node. You can access this by this [link](https://polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9945#/explorer)
