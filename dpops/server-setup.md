---
description: >-
  This guide is aimed at people not familiar with Linux servers or hosting
  services.
---

# Server Setup Guide

## Prerequisite

### Generate a SSH key

We recommend using public key authentication to access your server. Using a strong password to connect remotely in SSH is already secured, but using your own key pair for SSH connexion is always recommended.

The SSH key you will be generating will be used later on when using accessing your server.

#### On Windows 

One of the quickest way to generate a SSH key on Windows is by using PuttyGen. You can download Putty at [this link](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) and follow [this complete tutorial](https://www.ssh.com/ssh/putty/windows/puttygen) to create your SSH key pair.

#### **On Linux / Mac**

Open a terminal window on Linux or OSx and type the following command.

```bash
ssh-keygen -t rsa -b 4096
```

It will create a SSH key pair in the form `your_key.pub` and `your_key` created in your current directory. 

### Find a Linux Machine

#### System Requirements

{% hint style="info" %}
In the first beta version, X-Cash's DPoPS will only run on a Linux/Unix OS. **We recommend installing it on a Ubuntu VPS/dedicated server \(18.04\) for best compatibility.**
{% endhint %}

Before looking into a server hosting service, it is recommended that you acknowledge the system requirements for running a delegate node. 

The delegate node will need to transit a lot of information, notably messages to the other delegates to verify the block informations, As time goes by, the features and information that the delegates handle will increase \(notably when we will develop token creation, sidechains and other exciting features\). 

{% hint style="info" %}
The recommended system requirement is designed to be "future-development proof", meaning that a system update shouldn't be needed in the next few years and still comfortably handle the `xcash-dpops` program.
{% endhint %}

|  | **Minimum**  | **Recommended**  |
| :--- | :--- | :--- |
| **OS** | Ubuntu 18.04 \(or higher\) | Ubuntu 18.04 \(or higher\) |
| **CPU** | 2 threads, 2.0 GHz or more per thread  | 4 threads, 2.0 GHz or more per thread |
| **RAM** | 6GB | 8GB |
| **Hard Drive** | 50GB  | 100GB |
| **Bandwidth Transfer** | 100GB per month | 500GB per month |
| **Bandwidth Speed** | 100 Mbps | 500 Mbps |

{% hint style="warning" %}
 It is estimated that the blockchain size will increase by **18GB per year**.
{% endhint %}

#### Server hosting provider

There are a lot of server and VPS providers out there, notably **AWS**, **Google Cloud**, **Hetzner**, **Alibaba**, **Digital Ocean**, **OVH** etc... to only name a few. If you don't have your own server infrastructure, you will need to rent your server with a provider. 

We are agnostic as to which server provider you should choose to run the `xcash-dpops` program. Our recommendation is to follow the [system requirements](server-setup.md#find-a-linux-machine), and to follow the service you are most confortable with.

## Initialize your server

{% hint style="danger" %}
We will give a short tutorial here for preparing a server rented from [Hetzner](https://www.hetzner.com/). Those steps are easily replicated with any server hosting providers, and you shouldn't limit yourself to using Hetzner. We are also giving more general directions.
{% endhint %}

### Authentication

Use your SSH RSA key pair that you have [previously generated](server-setup.md#generate-a-ssh-key) to access your newly rented server. Most of the server hosting service lets you provide your SSH RSA key and automatically authorize your key, but if not you should indicate your key by yourself.

{% hint style="info" %}
In Hetzner, you can register your public SSH key in the **key management system**, and choose it when you run your server in rescue mode to install/re-install Ubuntu.
{% endhint %}

With the key pair you have [generated earlier](server-setup.md#generate-a-ssh-key), you will have to register the public key on your server so it can recognizes you. 

To do that, you need to copy the content of your public SSH key into the file `~/.ssh/authorized_keys` in your server. 







## Create a new user

adduser 

## Set up a custom domain name



