---
categories: ["Demo", "Tutorial", "Red Hat AMQ", "Running Locally"]
title: "Red Hat AMQ 7 HA Replication Failover [Demo]"
slug: "amq-7-replication-shared-nothing"
author: "Hugo Guerrero"
tags: ["amq", "redhat", "messaging", "amq7", "jboss"]
date: "2017-08-02"
avatarUrl: "hugo-guerrero.png"
---

A few weeks ago the newest version of [Red Hat AMQ was released](https://hguerreroo.wordpress.com/2017/05/04/jboss-amq-7-ga-announced-download-it-today/). AMQ 7 is the result of Red Hat’s efforts on creating an unified messaging  platform for it’s middleware offerings. One of the most interesting  features of this new version is the new backing strategies for  failovering when configured in high availability. This feature allows  clients connections to migrate from one server to another in event of  server failure so client applications can continue to operate.

AMQ 6.x already had and option to configure failover using a *shared store,* usually backed up by a shared filesystem or a jdbc connection to a  database. However, that option involved the use of external  infrastructure add-ons in hardware and software, representing an  increase in overall deployment costs.

In AMQ 7, support for network-based replication was addded. When  using replication, the live and the backup servers do not share the same data directories, all data synchronization is done over the network.  Therefore all (persistent) data received by the live server will be  duplicated to the backup.

![amq-ha-replicated](https://hguerreroo.files.wordpress.com/2017/08/amq-ha-replicated.png?w=1120)

The [Replication (Shared Nothing) Demo](https://github.com/jbossdemocentral/amq-ha-replicated-demo) project is a demonstration of the new AMQ 7 replicated high  availability feature to avoid using a shared store. This project  demonstrates how to set up a live broker and it’s corresponding backup  using AMQ’s command line interface (CLI) and configure the replication  ha failover policy.

You will need a Linux or Mac laptop with at least 2GB RAM (4GB  preferred) and a couple of GB of free disk space. A Java Virtual Machine version 8 should be installed in the system as well. With those  prerequisites covered you can deploy your brokers following the next  steps:

1. [Download and unzip this demo](https://github.com/jbossdemocentral/amq-ha-replicated-demo/releases/latest) of clone the [Github repository](https://github.com/jbossdemocentral/amq-ha-replicated-demo).

2. Download the JBoss AMQ Broker from Red Hat Developer Portal: —[download here](https://developers.redhat.com/products/amq/download/), add to installs directory (see installs/README).

3. Deploy the demo using the automated installation script ‘init.sh’

   ```sh
   ./init.sh
   ```

   After successfully deployed, you can test the failover.

#### Sending messages

To send messages to the master broker, execute the following command:

```sh
$ target/amq-broker-7.0.1/instances/replicatedMaster/bin/artemis producer --message-count 10 --url "tcp://127.0.0.1:61616" --destination queue://haQueue
```

#### Browse messages on Master

To check the messages were successfully send to the broker, check the queue in the broker web console.

- Open a web browser and navigate to the AMQ web console http://localhost:8161/hawtio
- In the left tree navigate to 127.0.0.1 > addresses > haQueue > queues > anycast > haQueue
- Click on *Browse* (refresh if necessary)

You will see the 10 messages send by the producer script.

#### Browse backup Console

As the replicatedSlave broker is running as a backup broker for  replicatedMaster, there are no active addresses or queues listening.

- Open a web browser and navigate to the AMQ web console http://localhost:8261/hawtio
- In the left tree navigate to 127.0.0.1 > addresses > haQueue > queues > anycast > haQueue

You will only see the information regarding the cluster broadcast configuration.

#### Master shutdown

To shutdown the master broker, execute the following command:

```sh
$ target/amq-broker-7.0.1/instances/replicatedMaster/bin/artemis-service stop
```

While the master is shutting down, the backup broker will notice the disconnection from the master and bwill become live.

#### Browse messages on Slave

To check the messages were successfully replicated to the slave broker, check the queue in the slave broker web console.

- Refresh the AMQ web console http://localhost:8261/hawtio
- In the left tree navigate to 127.0.0.1 > addresses > haQueue > queues > anycast > haQueue
- Click on *Browse* (refresh if necessary)

You will see the 10 messages send by the producer script to the master broker.

#### Failback

If you want, you can start again the replicatedMaster broker to see how the backup failbacks to the master.

```sh
$ target/amq-broker-7.0.1/instances/replicatedMaster/bin/artemis-service start
```

The master will start and check if there is a live broker, when the  backup detects that the master has become availbale again, it failsback  going in a backup mode again.

Now you are up and running with a fully installed and backed up AMQ 7 broker!

For more information, check the [Red Hat Developers Program](https://developers.redhat.com/products/amq/overview/) site that provides useful blogs and content about Red Hat JBoss AMQ.