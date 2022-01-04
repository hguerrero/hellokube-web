---
categories: ["Apache Kafka", "Containers", "Apache Zookeeper", "Tutorials"]
title: "Three easy ways to run Kafka without Zookeeper"
slug: "three-ways-zookeepeerless-kafka"
author: "Hugo Guerrero"
tags:
  [
    "kafka",
    "apache kafka",
    "zookeeper",
    "apache zookeeper",
    "kubernetes",
    "docker",
    "podman",
  ]
date: "2022-01-03"
avatarUrl: "hugo-guerrero.png"
draft: true
---

There has been a couple of years since the announcement of the removal of [Apache Zookeeper](https://zookeeper.apache.org/) as a dependency to manage [Apache Kafka](https://kafka.apache.org/) metadata. Since version 2.8, we now can run a Kafka cluster without Zookeeper. This article will go over three easy ways to get started with a single node cluster using [containers](https://www.redhat.com/en/topics/containers).

## Control and data planes

Apache Kafka implements independent control and data planes for its clusters. The control plane manages the cluster, keeps track of what brokers are alive, and takes action when the set changes. Meanwhile, the data plane consists of the features required to handle producers and consumers and their records. In the previous iterations, Zookeeper was the cluster component that held most of the implementation of the control plane.

After a couple of years of work, we have now the first releases implementing the new control plane, a change commonly known as [KIP-500](https://cwiki.apache.org/confluence/x/KoxiBw). [Apache Kafka Raft](https://cwiki.apache.org/confluence/display/KAFKA/KIP-595%3A+A+Raft+Protocol+for+the+Metadata+Quorum) (also known as KRaft) is the new consensus protocol introduced to replace Zookeeper. The brokers now can take the quorum controller role to manage the cluster control plane. This change simplifies the cluster deployment, monitoring, and administration. The new KRaft controller is available as early access as Apache Kafka 2.8.

## Containerized single node

Its ability to scale horizontally and manage high throughput are the most common drivers to implement Apache Kafka. For that reason, you will implement a cluster with many brokers for production usage. However, we will focus on a single node cluster for simplicity and a quick start.

Next, we will use containers to leverage the benefits of self-contained environments. We will be using the [Strimzi](https://strimzi.io/) project containers. Strimzi is a [Cloud Native Computing Foundation](https://www.cncf.io/) project member that provides an easy way to run Apache Kafka on Kubernetes with a mature set of operators and container images. The Strimzi team is working on the Zookeeper removal in their operators. Keep track of their future work in this area if you are interested.

For this article, we will be using Apache Kafka version 2.8.1 images published and available in the [Quay Container Registry](https://quay.io/).

## Docker or Podman

The first option is to run a single container with both the broker and the controller roles in the same instance. We just need to have [Docker](https://www.docker.com/) or [Podman](https://podman.io/) installed and run the following command:

```sh
docker run -it --name kafka-zkless -p 9092:9092 -e LOG_DIR=/tmp/logs quay.io/strimzi/kafka:latest-kafka-2.8.1-amd64 /bin/sh -c 'export CLUSTER_ID=$(bin/kafka-storage.sh random-uuid) && bin/kafka-storage.sh format -t $CLUSTER_ID -c config/kraft/server.properties && bin/kafka-server-start.sh config/kraft/server.properties'
```

The above command starts a container named `kafka-zkless` and exposes the standard Kafka port 9092. We override the entry point and run three orders in one single line. The first one sets an environment variable that creates a random UUID for the cluster id. The second part runs the `kafka-storage.sh` script to format the storage directory. Finally, the last function starts the Kafka server using the KRaft configuration.

You can connect to the broker using any standard Kafka tooling like [kcat](https://github.com/edenhill/kcat) (formerly known as kafkacat) to produce and consume records.

## Docker Compose

Now, if you rather prefer to run Kafka as part of a more complex setup, you can use the [Compose specification](https://compose-spec.io/) to define your components. Here is an example of a docker-compose YAML file:

```yaml
version: "2"
services:
  kafka:
    container_name: kafka
    image: quay.io/strimzi/kafka:latest-kafka-2.8.1-amd64
    command:
      [
        "sh",
        "-c",
        "export CLUSTER_ID=$$(bin/kafka-storage.sh random-uuid) && bin/kafka-storage.sh format -t $$CLUSTER_ID -c config/kraft/server.properties && bin/kafka-server-start.sh config/kraft/server.properties --override advertised.listeners=$${KAFKA_ADVERTISED_LISTENERS} --override listener.security.protocol.map=$${KAFKA_LISTENER_SECURITY_PROTOCOL_MAP} --override listeners=$${KAFKA_LISTENERS}",
      ]
    ports:
      - "9092:9092"
    environment:
      LOG_DIR: "/tmp/logs"
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_LISTENERS: PLAINTEXT://:29092,PLAINTEXT_HOST://:9092,CONTROLLER://:9093
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
```

You can run the above example by issuing the following command:

```sh
docker-compose up -d
```

It exposes port 9092 of the host system for the bootstrap server. If you need to access it as part of a more extensive compose deployment, you can use kafka:29092 instead.

## Kubernetes deployment

Finally, if you are already using any flavor of Kubernetes, you can take a look at the following deployment descriptor:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: zkless-kafka
spec:
  selector:
    matchLabels:
      app: zkless-kafka
  template:
    metadata:
      labels:
        app: zkless-kafka
    spec:
      containers:
        - name: zkless-kafka
          image: quay.io/strimzi/kafka:latest-kafka-2.8.1-amd64
          command:
            - /bin/sh
            - -c
            - "export CLUSTER_ID=$(bin/kafka-storage.sh random-uuid) && bin/kafka-storage.sh format -t $CLUSTER_ID -c config/kraft/server.properties && bin/kafka-server-start.sh config/kraft/server.properties --override advertised.listeners=${KAFKA_ADVERTISED_LISTENERS}"
          env:
            - name: LOG_DIR
              value: /tmp/logs
            - name: KAFKA_ADVERTISED_LISTENERS
              value: PLAINTEXT://zkless-kafka-bootstrap:9092
          resources:
            limits:
              memory: "128Mi"
              cpu: "500m"
          ports:
            - containerPort: 9092
```

You can apply the previous file using the following command:

```sh
kubectl apply -f kubernetes.yaml
```

The previous command will create a deployment that will expose the bootstrap server using the `zkless-kafka-bootstrap` hostname. It will be limiting access to the current namespace where the deployment resides. You can play with the advertised listeners to tweak it to your needs.

## Summary

This article took a peek into the new control plane implementation for Apache Kafka. Significant work has been accomplished in the last version, and we are now able to play with a functional Zookeeperless Kafka cluster. We reviewed three easy ways to try a single node cluster using container images: running a simple docker or podman command, running docker-compose, or deploying a running pod on Kubernetes.

You can check the commands and files in my [GitHub repository](https://github.com/hguerrero/amq-examples/tree/master/zkless-kafka), find more examples for Apache Kafka and Red Hat AMQ streams, and more information. Finally, don’t forget to follow on [Twitter](https://twitter.com/hguerreroo/) for more content on APIs and Event-driven Architecture.
