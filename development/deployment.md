---
description: >-
  SyndLend node can be deployed in several ways. It is up to the consumer to
  select the best deployment strategy in their existing infrastructure.
---

# Deployment

## Deploying Corda nodes using docker

### 1. Install Docker in Ubuntu

**Docker 17.09 and up is required for this Dockerfile.**

Install docker-ce following this reference: [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

After docker installed, `sudo docker -v` should display docker's version

### 2. Git clone this repository to your workspace

```text
$ git clone https://github.com/consensolabs/syndlend-devops.git
```

### 3. Bootstrapping network for corda

The nodes see each other using the network map. This is a collection of statically signed node-info files, one for each node in the network. Most production deployments will use a highly available, secure distribution of the network map via HTTP.

In addition to the network map, all the nodes on a network must use the same set of network parameters. These are a set of constants which guarantee interoperability between nodes. The HTTP network map distributes the network parameters which the node downloads automatically

```text
$ java -jar corda-3.3/network-bootstrapper.jar nodes
```

the command will generate nodes' directory for us as below:

```text
$ tree -L 2 nodes
nodes
├── Agent
│   ├── additional-node-infos
│   ├── certificates
│   ├── corda.jar
│   ├── cordapps
│   ├── logs
│   ├── network-parameters
│   ├── node.conf
│   ├── nodeInfo-2224690BD0A834EDE0202AFF4A4EA2624BB043DA4464C5706203901F9E91F766 #generated
│   └── persistence.mv.db
├── Borower
│   ├── additional-node-infos
│   ├── certificates
│   ├── corda.jar
│   ├── cordapps
│   ├── logs
│   ├── network-parameters
│   ├── node.conf
│   ├── nodeInfo-96AADFD07507CB2398D111DCAA350A6874599F1FEAB9207D09F0224D9829839B
│   └── persistence.mv.db
├── Notary
│   ├── additional-node-infos
│   ├── certificates
│   ├── corda.jar
│   ├── cordapps
│   ├── logs
│   ├── network-parameters
│   ├── node.conf
│   ├── nodeInfo-65B670371BB5F63D25527A3C515AABFBE0B1380D11EB9DB82563E53012A232BA
│   └── persistence.mv.db
├── Lender
│   ├── additional-node-infos
│   ├── certificates
│   ├── corda.jar
│   ├── cordapps
│   ├── logs
│   ├── network-parameters
│   ├── node.conf
│   ├── nodeInfo-65B670371BB5F63D25527A3C515AABFBE0B1380D11EB9DB82563E53012A232BA
│   └── persistence.mv.db
└── whitelist.txt
```

### 4. Using docker compose

* Check Dockerfile and docker-compose.yml \(e.g. to adjust version or exposed ports\)
* `docker-compose build` - to build base Corda images for Corda Node
* `docker-compose up` - to spin up all Corda containers
* `docker exec -it Agent /bin/bash` - to log in to one of the running Node containers

### Corda docker configuration

At the moment java options are put into **corda\_docker.env**. All the others are in Dockerfile/docker\_compose.yml

### Interaction

You should be able to access all the RPC ports exposed by the nodes through docker containers. You can go ahead and launch the [SyndLend client application](https://github.com/consensolabs/syndlend-client).

{% page-ref page="syndlend-client.md" %}

