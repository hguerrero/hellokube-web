---
categories: ["Kubernetes", "Docker"]
title: "Three ways to run Kubernetes in your laptop with Docker"
slug: "3-ways-to-run-kubernetes-on-docker"
author: "Hugo Guerrero"
tags: ["kubernetes", "docker", "minikube","kind","k3d"]
date: "2021-04-09"
avatarUrl: "hugo-guerrero.png"
---

In the past, running a Kubernetes cluster on your laptop required several servers to get started or running heavyweight Virtual Machines in your local environment. Different projects began in recent years to make it easier to get started with Kubernetes in your computer and remove the dependency with VMs. This post will cover 3 of them to help you o get started with Kubernetes on your laptop just by using Docker.

First of all, Kubernetes is an open source platform to manage workloads by abstracting the underlying infrastructure. It helps you manage microservices containers and makes them portable across clouds, including your laptop. As a developer, you may care about Kubernetes because it is the defacto standard to deploy and manage containerized applications.

In [my journey to make my Windows machine usable for development](https://hellokube.dev/posts/developers-journey-to-wsl2-awesomeness/), I searched for options to remove the Kubernetes dependency of a Virtual Machine. I found the easiest way to achieve this would be by running Kubernetes *on top* of Docker. I know it sounds like Inception, but believe me! It was the easiest way to get started on a laptop with medium-level resources. 

At this time, the three main ways I have found to run Kubernetes on Docker are [KinD](https://kind.sigs.k8s.io/), [Minikube](https://minikube.sigs.k8s.io/docs/start/) with the new docker driver, and [K3d](https://k3d.io/). KinD was the first one I tried before Minikube added their new driver. Finally, after some hesitance, I tried k3d to find it very useful. Let me explain further and give a review of each one of them.

## KinD

KinD (Kubernetes in Docker) was the result of Kubernetes testing itself. It is a tool to run clusters locally, focusing on development and CI. It is a tool that allows you to configure it to run from one node cluster to several nodes. It is the Do It Yourself (DIY) version and, because of that, users might find that some pieces are missing or require separate assembly. 

KinD does not provide an easy way to set up an ingress. If you need to access the cluster’s services and not just test within it, you will need to create a particular configuration file and install a supported Ingress controller. If you need a registry, you will need to deploy independently and connect it to your cluster. 
Customization is a double-edged sword. You might want to be able to use your tools and configurations if you become a Kubernetes master. Most of the time, as a developer, you just want to get started, and having to move all the knobs can get frustrating.

You can follow the KinD [quickstart](https://kind.sigs.k8s.io/docs/user/quick-start/) to get started.

## Minikube

The Kubernetes team’s goal was to “*build an easy-to-use, high-fidelity Kubernetes distribution that can be run locally on Mac, Linux, and Windows workstations and laptops with a single command,”* according to their 2016 [blog](https://kubernetes.io/blog/2016/07/minikube-easily-run-kubernetes-locally/) with the release of version 1.3.

In the beginning, Minikube required a virtualization engine to create a Virtual Machine where it installs the Kubernetes cluster. Today, the new [Docker driver](https://minikube.sigs.k8s.io/docs/drivers/docker/) is the preferred way to go as it allows you to install Kubernetes into your existing Docker. Even though the support for WSL2 on Windows 10 is considered experimental, I can validate that it is pretty usable with [some minor hacks](https://hellokube.dev/posts/configure-minikube-ingress-on-wsl2/). 

Minikube is the arrowhead of the Kubernetes community, and because of that, they try to add all the latest improvements as quickly as possible. The addons ecosystem is another benefit of this distribution. Addons are maintained extensions to minikube that add functionality to the base cluster.

Adding features like an Ingress is super easy. You just need to enable the suggested addon. You can add other components in the same way with tools like a dashboard or a registry and others. It is a good set of options available, a la carte.  

## K3d

As their project page states, k3d is a lightweight wrapper for Rancher Lab’s distribution of Kubernetes focused on edge. It is hence, a very opinionated way to run your cluster. On the other side, it provides out-of-the-box most of the elements expected from a complete, usable solution like the [Traefik ingress](https://doc.traefik.io/traefik/providers/kubernetes-ingress/). 

The latest v4 release simplified the way to integrate with Kubectl, making it easier to access the cluster without messing up your configurations. It makes it very straightforward to create multi-server clusters by just setting a flag while creating your cluster. 

Finally, an essential part for me was the start-up time. It was swift. And the number of resources used was way lower compared with the other two options. The tuning made by Rancher to remove dependencies on Etcd and other components is pretty palpable.

## Summary

All three options offer a good way for you to get started with Kubernetes without the hassle of creating a Virtual Machine. They inherit the benefits of containers, like immutability and a quick startup time, making them a more suitable tool for deploying your application in an environment similar to production. Depending on the customization level, you might prefer the versatility of kind, the vast catalog of addons for Minikube, or the opinionated way from K3d. 

With any option, you will get started more accessible and quicker in your Kubernetes journey. Which one did you use?