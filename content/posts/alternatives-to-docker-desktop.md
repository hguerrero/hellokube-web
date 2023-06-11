---
categories: ["Containers", "Docker"]
title: "Alternatives to Docker Desktop"
slug: "alternatives-to-docker-desktop"
author: "Hugo Guerrero"
tags: ["containers", "docker", "rancher desktop","podman"]
date: "2022-02-09"
avatarUrl: "hugo-guerrero.png"
---

Last year Docker[ announced a change in their subscription service agreement](https://www.docker.com/blog/updating-product-subscriptions/) that limited the free usage of[ Docker Desktop](https://www.docker.com/products/docker-desktop) effective August 31, 2021, with a grace period until January 31, 2022. The grace period is over, so what are your options if you don't fall in any of the allowed categories to keep it running for free or if you just want to look for alternatives? In this post, we will go over podman and rancher desktop to check if they can replace Docker Desktop usage.

A lot of ink has run regarding the goals behind the change in the product subscription, and I won't be able to cover them in this article, so I will just summarize the actual changes. First, Docker Desktop is still free for personal use, open source projects, and small businesses. The real difference comes for subscribers that use it for professional work. Docker sets the barrier at 250 employees and $10 million in annual revenue. If your employer is above those limits, you will need a professional plan starting at $5 per user per month to comply.

Although this could be something you can cover as part of your monthly expenses, you might find this is an excellent moment to look for alternatives. Depending on your requirements, time, and interest in trying new things, you will find a couple of options that might fulfill your curiosity. Let’s go over the ones I found and was able to try for a couple of scenarios I usually work with my demos.

### Podman

The Pod Manager Tool ([podman](https://podman.io/)) is a project sponsored by Red Hat to build a better set of tools around containers. According to the project’s page, “Podman is a daemonless container engine for developing, managing, and running OCI Containers on your Linux System.” As you can see by this definition, the main target was Linux systems. However, most developers are not running Linux on their laptop machines. Podman has been working to improve the experience on Mac and Windows with the help of virtualization. Brent Baude has a great article on the sysadmin site on [how podman runs on Macs](https://www.redhat.com/sysadmin/podman-mac-machine-architecture). 

When running podman on non-Linux systems, you can notice the lack of a user interface to interact with the tooling. The only way to work with it is by using the terminal and command line, so you must feel comfortable with that. The second thing you will notice is the clear presence of the virtual machine used to run podman. The virtual machine presence is a big difference from Docker Desktop, where they do a great job hiding it from the user experience. After getting the machine started, you can start using the podman CLI. You will be able to issue similar commands to Docker, like pull, push and run for your containers. 

Unfortunately, it starts to get complicated if you need to run something more complex than an isolated container, like a docker-compose file with a couple of containers. There is no native support for docker-compose.yaml files within podman. You can try to migrate your workload to kubernetes.yaml style files for your content, but most of what you will find around GitHub won’t be in that format. A team created an independent project to [add a script to run docker-compose files using podman](https://github.com/containers/podman-compose). Still, I could not wholly run all my workloads even with this script. It looks like the support for a shared filesystem with the host and networking still needs some work. Some alternatives mentioned in the GitHub repo required further configuration to access the podman socket, but I didn’t go that far.

### Rancher Desktop

During the turmoil, [Rancher](https://rancher.com/) released version 1.0 of the [Rancher Desktop](https://rancherdesktop.io/) project targeting easy the management of Kubernetes and containers on the Desktop. They have native binary installers for Mac, Windows, and Linux. It is based on Rancher’s software stack distribution of Kubernetes and initially targeted its adoption. However, they update the solution to manage single containers too. 

Rancher has a valuable but straightforward graphic interface to configure the virtual machine that runs either the containerd or dockerd daemons as part of their Kubernetes distribution. This point is important because if you configure dockerd as the engine, you get support for the docker CLI directly without any additional configuration. If you decide to enable containerd, Rancher Desktop installs the [nerdctl CLI](https://github.com/containerd/nerdctl) to interact with it. You will get similar commands to run, pull and execute containers. 

Now, on the docker-compose support, I can say that Rancher Desktop presented less friction to run my workloads. No need to do any extra configuration to run my yaml files. Also, there were no issues mounting files or connecting through the network. I just had to turn off Docker Desktop and start Rancher Desktop. I continued issuing the same commands and got the same behavior as before. It was a joy.

### Docker Desktop

Well, yes! Keep running Docker Desktop is one of your alternatives. I went over a couple of options that require you to download files or configure additional machines on your laptop. But, you can also spend those $5 and keep it as it is—no additional burden. You can always consider it as an alternative too.

## Summary

Have $5 to spend monthly? You can continue using Docker Desktop. Do you want to avoid the fee? There are a couple of alternatives that can replace Docker Desktop with a free solution. Podman and Rancher Desktop are two options that check most of the boxes. They have their pros and cons, and it will depend on what you are looking to achieve. I found that Rancher Desktop offered the most seamless replacement in my case. Did you find something more accessible? Let me know!