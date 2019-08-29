---
description: >-
  SyndLend node can be deployed in several ways. It is up to the consumer to
  select the best deployment strategy in their existing infrastructure.
---

# Deployment

{% hint style="info" %}
Although the source code of SyndLend core is private, an artifact of SyndLend nodes can be found [here](http://jenkins.consensolabs.com:8080/job/deploy-syndlend-corda-nodes/lastSuccessfulBuild/artifact/).
{% endhint %}

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



## Deploying a node to a server

{% hint style="info" %}
These instructions are intended for people who want to deploy a Corda node to a server, whether they have developed and tested a CorDapp following the instructions in generating-a-node or are deploying a third-party CorDapp.
{% endhint %}

### Linux: Installing and running Corda as a system service

We recommend creating system services to run a node and the optional webserver. This provides logging and service handling, and ensures the Corda service is run at boot.

**Prerequisites**:

> * A supported Java distribution. The supported versions are listed
>
>   in getting-set-up

1. As root/sys admin user - add a system user which will be used to run Corda:

   > `sudo adduser --system --no-create-home --group corda`

2. Create a directory called `/opt/corda` and change its ownership to the user you want to use to run Corda:

   `mkdir /opt/corda; chown corda:corda /opt/corda`

3. Download the [Corda jar](https://r3.bintray.com/corda/net/corda/corda/) \(under `/|corda_version|/corda-|corda_version|.jar`\) and place it in `/opt/corda`
4. \(Optional\) Download the \[Corda webserver

   jar\]\([http://r3.bintray.com/corda/net/corda/corda-webserver/](http://r3.bintray.com/corda/net/corda/corda-webserver/)\) \(under

   `/|corda_version|/corda-|corda_version|.jar`\) and place it in

   `/opt/corda`

5. Create a directory called `cordapps` in `/opt/corda` and save your

   CorDapp jar file to it. Alternatively, download one of our \[sample

   CorDapps\]\([https://www.corda.net/samples/](https://www.corda.net/samples/)\) to the `cordapps`

   directory

6. Save the below as `/opt/corda/node.conf`. See corda-configuration-file for a description of these options:

   ```text
   p2pAddress = "example.com:10002"
   rpcSettings {
       address: "example.com:10003"
       adminAddress: "example.com:10004"
   }
   h2port = 11000
   emailAddress = "you@example.com"
   myLegalName = "O=Bank of Breakfast Tea, L=London, C=GB"
   keyStorePassword = "cordacadevpass"
   trustStorePassword = "trustpass"
   devMode = false
   rpcUsers= [
       {
           user=corda
           password=portal_password
           permissions=[
               ALL
           ]
       }
   ]
   custom { jvmArgs = [ '-Xmx2048m', '-XX:+UseG1GC' ] }
   ```

7. Make the following changes to `/opt/corda/node.conf`:

   * Change the `p2pAddress`, `rpcSettings.address` and

     `rpcSettings.adminAddress` values to match your server's

     hostname or external IP address. These are the addresses other

     nodes or RPC interfaces will use to communicate with your node.

   * Change the ports if necessary, for example if you are running

     multiple nodes on one server \(see below\).

   * Enter an email address which will be used as an administrative

     contact during the registration process. This is only visible to

     the permissioning service.

   * Enter your node's desired legal name \(see node-naming for more

     details\).

   * If required, add RPC users

8. **SystemD**: Create a corda.service file based on the example

   below and save it in the /etc/systemd/system/ directory



   1. > ```text
      > [Unit]
      > Description=Corda Node - Bank of Breakfast Tea
      > Requires=network.target
      >
      > [Service]
      > Type=simple
      > User=corda
      > WorkingDirectory=/opt/corda
      > ExecStart=/usr/bin/java -jar /opt/corda/corda.jar
      > Restart=on-failure
      >
      > [Install]
      > WantedBy=multi-user.target
      > ```

9. **Upstart**: Create a `corda.conf` file based on the example below and save it in the `/etc/init/` directory

   > ```text
   > description "Corda Node - Bank of Breakfast Tea"
   >
   > start on runlevel [2345]
   > stop on runlevel [!2345]
   >
   > respawn
   > setuid corda
   > chdir /opt/corda
   > exec java -jar /opt/corda/corda.jar
   > ```

10. Make the following changes to `corda.service` or `corda.conf`:

    > * Make sure the service description is informative -
    >
    >   particularly if you plan to run multiple nodes.
    >
    > * Change the username to the user account you want to use to run
    >
    >   Corda. **We recommend that this user account is not root**
    >
    > * **SystemD**: Make sure the `corda.service` file is owned by root with the correct permissions:
    >
    >   > * `sudo chown root:root /etc/systemd/system/corda.service`
    >   > * `sudo chmod 644 /etc/systemd/system/corda.service`
    >
    > * **Upstart**: Make sure the `corda.conf` file is owned by root with the correct permissions:
    >
    >   > * `sudo chown root:root /etc/init/corda.conf`
    >   > * `sudo chmod 644 /etc/init/corda.conf`

### Interaction

You should be able to access all the RPC ports exposed by the nodes through docker containers. You can go ahead and launch the [SyndLend client application](https://github.com/consensolabs/syndlend-client).

{% page-ref page="syndlend-client.md" %}



