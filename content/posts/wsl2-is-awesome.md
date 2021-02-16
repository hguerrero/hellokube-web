---
categories: ["WSL2", "Windows", "Development"]
title: "A Developer's Journey to WSL2 Awesomeness"
slug: "developers-journey-to-wsl2-awesomeness"
author: "Hugo Guerrero"
tags: ["wsl2", "windows", "linux", "docker", "vscode","development"]
date: 2021-02-15T09:21:51-05:00
avatarUrl: "hugo-guerrero.png"
---

The shift in modern systems architecture to cloud and microservices brought huge changes to the way people develop applications. A long time ago (~15 years) we focused on clients accessing directly the database or some middleware running on local servers. In that time, the development environment had to reflect the target environment where those applications were going to be running. As you can imagine, most of the clients were targeting hardware running Windows. Hence, having a Windows PC/laptop was quite useful and, let's admit it, there were not many other options to chose from. However, nowadays, running applications targeting big cloud providers and devices based on Unix/Linux operating systems made it difficult for Windows users to properly emulate those systems.

## A developer's journey

I was part of those Windows users from the 2000s, developing .NET applications and Java clients. My first approach to Unix-like systems came as part of my role as a developer that had to collect logs from the AIX server where the application server was running to check what went wrong. So, I had to install an SSH and Telnet client in my Windows laptop to get access to such servers. In those days, there was no concept of DevOps and most of the AIX knowledge I had came from blind-trusted commands provided by the sysadmin. The change of the backend servers to Linux, only affected a couple of commands so the knowledge was transferable.

Years later, somehow I ended working for the biggest Linux company, but on the middleware applications side. Suddenly, I received an enterprise Linux (RHEL) machine as my day to day laptop. Overnight, I had to ramp up my sysadmin skills to do simple everyday tasks. Fortunately, the company provided a lot of enablement material and access to world experts that were kind to help. I review the content, took the training and my Linux skills become good enough to solve the most common user problems. I must admit, I never went the full sysadmin path as I understand that is a long journey, 

After that, I switched roles in the same company and now had to work with a focus on devs riding the tip of the tech wave and switched to the most friendly Unix-based operating system disguised under a pretty UI. Switching between Unix and Linux is not that painful and the retraining takes a short time. I was still productive, now with the benefit of having a bigger ecosystem for everyday use. 

At home, my wife’s work required to interact with very legacy applications that made it almost impossible to switch her to Linux or Mac. We bought a Windows laptop with a decent amount of resources targeting her use cases as well as some retro pc games. From time to time, I tried to use her laptop to do some quick hacks to things like personal blogs and some side pet projects.

Oh my! That was painful! Installing my normal tooling and languages was a complete mess. Package manager? Obviously no! If so, that was difficult to configure and manage. Installing everything was complicated and error-prone.  Trying to use container or virtualization software required me to acquire a Windows Pro license to get access to features available out of the box in other operating systems. Furthermore, the resource usage of such virtual machines was overkill for average laptops. I did gave up and forget to use windows for a while. 

Fast forward some months, the need to play some old PC video games was the perfect opportunity to buy a new more beefy laptop. This time, the machine would be equipped with enough resources for gameplay and, as a consequence, to get back to trying to use it as a development machine. Surprisingly, the experience was different from my previous attempt. 

## WSL2 is here, ditch WSL 1

I must admit that I tried the Windows Subsystem for Linux (WSL) in my first attempt and I was not impressed. However, at that time, WSL2 was already out.  And even though there were only a few articles on how to set up and configure, most of them were praising the new version. The most interesting thing I found, while trying to setup and configure my environment, was that docker was launching support for the WSL2 subsystem. It certainly caught my eye.

I am not a fan of PowerShell, and being one of the suggested ways to install and configure WSL2, you can imagine I was not successful the first time. Eventually, I was able to enable the Virtual Machine platform and version 2 of the subsystem. Then, was time to choose a distribution to install. My first pick was Alpine, a tiny distribution I was interested to try. Rapidly I realize I needed something more close to traditional distribution. As you can imagine, I was surprised that a well known and used distribution like Fedora was not available. Time to try the second-best option hence, Ubuntu started to install. Finally, the revelation: this is a very good Linux impersonation running on Windows!

I installed the new Windows Terminal to replace the old fashioned CMD.exe and PowerShell clients. I was surprised by the comfortable feeling that it brought. This was closer to the well-known iTerm2 on macOS or the Linux terminal I was used to. I started to install my regular software packages, like the OpenJDK, Node.js, among others. The process went like a breeze for most of it. I was even able to access my Windows files from within Linux and my Linux files from Windows!

## Beware not to get bitten

The experience was so seamless for me, that I even forgot I was on a Windows machine. Until then expected issues started to appear. You need to be very aware that the subsystem for Linux is not a fully-fledged install or virtual machine. Hence, you won’t find certain features related to the underlying OS management. Some of the tooling applications I was trying to install, like the snap store, required access to the systemd dependency. I had to work around and install manually such applications. 

In addition to the manual install, I realize that the WSL2 does not include an X server. Resulting in the lack of graphical applications or a desktop. I can understand this, as you can use the main Windows system for this. However, not knowing how the integration with Windows applications and the Windows filesystem worked, worried me a bit. 

Despite my lack of knowledge, the VS Code plugins and the Docker for Desktop integrations for WSL2 showed that apps are getting ready for this new environment. In the case of VS Code, its plugins allow you to have it as a Windows application that can access the WSL2 filesystem and open the embedded terminal to connect to Linux. Docker has an option the make use of the WSL2 integration and thus, avoiding the creation of a virtual machine to run containers. Finally! Something useful to do some development using a Windows machine!

## My take on coding with WSL2 as a developer

I was greatly surprised by the huge leap forward Microsoft has taken to catch on developer’s joy. For the first time in a while, I can easily switch between my PC gaming mode and my side project development on the same laptop. I can do it without the need to restart for the dual boot or to waste a ton of resources running a virtual machine to have the same environment as the one my applications run on. I know it is still a long road ahead, but I do think they are heading in the correct direction. Making it easier for developers to interact with Linux from their Windows laptops will help to have more cloud-native applications and the possibility to expand those developer’s career options beyond the traditional paths.

I tell you: update your Windows to the latest version, enable the Windows Subsystem for Linux, set version 2 as default, install an easy to use Linux distribution, add your development applications, and start coding without the limitations we used to have!
