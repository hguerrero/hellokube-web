---
categories: ["WSL2", "Windows", "Kubernetes"]
title: "Configuring Ingress to run Minikube on WSL2 using Docker runtime"
slug: "configure-minikube-ingress-on-wsl2"
author: "Hugo Guerrero"
tags: ["wsl2", "windows", "linux", "docker", "minikube", "ingress"]
date: "2021-02-22"
avatarUrl: "hugo-guerrero.png"
---

As I mentioned previously, I’m trying to ramp up my Kube development using Windows’ newest Windows Subsystem for Linux 2 (WSL2). You can read my [previous post](https://hellokube.dev/posts/developers-journey-to-wsl2-awesomeness/) on how I found it remarkable to use my gaming laptop for day-to-day development. However, using WSL2 with Docker and Minikube brought not few challenges to make it work smoothly. In this post, I will cover how to make the Nginx Ingress controller work with Minikube when creating a cluster that is running on Docker instead of the traditional virtual machine.


If you remember, one of my goals was to be able to run my containers without having to create a virtual machine and spare some resources. [Docker for Desktop announced WSL2 support](https://docs.docker.com/docker-for-windows/wsl/) since version 2.3.0.2 with some simple requirements like run Windows 10 version 1093 or higher and having WSL 2 enabled. If you didn’t select the option when installing Docker, you could change the configuration of the General settings.

![Docker for Desktop General Settings](/images/docker-wsl2.png)

It will default to the same distribution you have configured to use with WSL2, but you can change it in the WSL section of the Resources settings. Enabling the WSL2 support will make Docker accessible from your Linux distribution.

# Setting up Minikube

If you haven’t done so, install the minikube following the Linux instructions from their [start page](https://minikube.sigs.k8s.io/docs/start/). Remember that you will be running all the commands from the WSL2 environment, so don’t forget to use the terminal to connect to your Linux distribution!


To run Minikube directly using the Docker runtime, you need to select the `docker` driver when starting the cluster.

`$ minikube config set driver docker`

This command will set up docker as the default environment to run minikube. If you require it, you can add more CPU and memory as the preselected 2GB setting could be a bit low for your usage.

You should see something similar to the following message after starting minikube.

```sh
😄  minikube v1.14.2 on Ubuntu 18.04
...
✨  Using the docker driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🔥  Creating docker container (CPUs=4, Memory=8192MB) ...
🐳  Preparing Kubernetes v1.19.2 on Docker 19.03.8 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" by default
```

Now, here comes the tricky part. 

# Enabling Ingress

If you follow the instructions from the minikube site that ask you to `enable` the ingress addon, you will face either that the addon is not supported or uses the internal WSL2 IP for the LoadBalancer. Enabling the ingress addon will not work, as the IP used is not accessible from your Windows environment. Hence, there will be no way to access your web applications or API endpoints from the windows browser. As I mentioned before, WSL2 does not come with a graphical environment, as it makes no sense to duplicate the already Windows UI available. So, if you want to debug your applications, you will need another way to configure your ingress.

The easiest way I overcame the minikube addon mismatch problems is to use the `cloud` based install of the ingress. In my case, I will be using the Nginx ingress controller for this. You will need to follow the Docker for Mac instructions because you face similar challenges as the Mac users. 

> Remember to `disable` the ingress addon before installing the Nginx ingress controller! 

```sh
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/deploy/static/provider/cloud/deploy.yaml
```

This command will create the `nginx-ingress` namespace and install all the required resources to set up the controller. 

If you check the services in your cluster, you will find that the `ingress-nginx-controller` service has the `LoadBalancer` type and is `<pending>` the external IP. The pending IP is because the cloud configuration expects a cloud provider to provision a real Load Balancer.

Minikube offers the option to expose the `LoadBalancer` services via the `minikube tunnel` command to solve this pending issue. You will need to execute and keep running this command in another window while using the ingress service.

Because this command configures the service to listen in privileged ports, you will need to have an administrative role in the Linux system to allow it. If required, it automatically will prompt you for sudo. You should see something similar to this:

```sh
❗  The service ingress-nginx-controller requires privileged ports to be exposed: [80 443]
🔑  sudo permission will be asked for it.
🏃  Starting tunnel for service ingress-nginx-controller.
[sudo] password for user1:
```

# Testing the configuration

Now that the tunnel is working, you will see that the `External-IP` for the ingress service is 127.0.0.1, the same as `minikube ip`. You can now test the configuration by accessing using curl or your favorite HTTP client. 

If you try to connect from your browser to http://localhost:80, you will see something similar to the following screen.

![Ingress access from the Windows browser](/images/localhost-ingress.png)

You should be able to reach the ingress controller from minikube. However, the result will be a 404 HTTP error because we haven’t defined any ingress to resolve the `localhost` hostname.

You can follow the [tutorial ](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)from minikube to deploy and test a simple application using the ingress custom resource.

# Summary

The new features of WSL2 enable other provider’s applications to use the resources on local machines better. One clear example of this is the Docker for Desktop running directly on WSL2. Without the need to create an additional virtual machine to run containers, you can now run minikube using the docker driver from within your Linux distribution. 

However, the way the subsystem handles the networking creates challenges to set up the Ingress controller. You can work around it if you install manually the ingress controller, for example, the Nginx one, instead of enabling the ingress addon included with minikube. The tunnel command can then help the required LoadBalancer service to listen in the HTTP and HTTPS ports. With this service running, you can now access your ingress services directly from the Windows browser. 