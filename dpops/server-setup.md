---
description: >-
  This guide is aimed at people not familiar with Linux servers or hosting
  services.
---

# Server Setup Guide

## Introduction

Generally speaking, running a delegate node requires knowledge and experience in information and network technologies. However, we believe that it should be accessible to everyone who is willing to learn. This guide will help you get introduced to servers and Linux system, and getting started securely to start your own delegate node.  
If you find yourself stranded, [our helpful community](https://discord.gg/4CAahnd) will be happy to help you 🙂

## Prerequisite

### Generate an SSH key

We recommend using public-key authentication to access your server. Using a strong password to connect remotely in SSH is already secured, but using your own key pair for SSH connexion is always recommended.

The SSH key you will be generating will be used later on when accessing your server.

#### On Windows

One easy way to generate an SSH key on Windows is by using PuttyGen. You can download Putty at [this link](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) and follow [this complete tutorial](https://www.ssh.com/ssh/putty/windows/puttygen) to create your SSH key pair.

#### **On Linux / Mac**

Open a terminal window on Linux or OSx and type the following command.

```bash
ssh-keygen -t rsa -b 2048
```

It will generate an SSH key pair in the form of a public key `id_rsa.pub` and the corresponding private key `id_rsa` created in the `~/.ssh` directory

### Find a Linux Machine

#### System Requirements

{% hint style="info" %}
In the first beta version, X-Cash's DPoPS will only run on a Linux/Unix OS. **We recommend installing it on a Ubuntu VPS/dedicated server \(18.04\) for the best compatibility.**
{% endhint %}

Before looking into a server hosting service, it is recommended that you acknowledge the system requirements for running a delegate node.

The delegate node will need to transmit a lot of information, notably messages to the other delegates to verify the block informations, As time goes by, the features and information that the delegates handle will increase \(notably when we will develop **token creation**, **NFT**, **sidechains**, **smart contracts** and other exciting features\).

{% hint style="info" %}
The recommended system requirement is designed to be "**future-development proof**", meaning that a hardware update should never be needed and it will still comfortably handle the `xcash-dpops` program.
{% endhint %}

|  | **Minimum** | **Recommended** |
| :--- | :--- | :--- |
| **OS** | Ubuntu 18.04 | Ubuntu 18.04 |
| **CPU** | 4 threads, 2.0 GHz or more per thread | 4 threads, 2.0 GHz or more per thread |
| **RAM** | 6GB | 32GB |
| **Hard Drive** | 50GB | 2TB |
| **Bandwidth Transfer** | 100GB per month | 500GB per month |
| **Bandwidth Speed** | 100 Mbps | 500 Mbps |

{% hint style="warning" %}
It is estimated that the blockchain size will increase by **9GB per year.**
{% endhint %}

#### Server hosting provider

There are a lot of server and VPS providers out there, notably **AWS**, **Azure**, **Google Cloud**, **Hetzner**, **Alibaba**, **DigitalOcean**, **OVH** etc... to only name a few. If you don't have your own server infrastructure, you will need to rent your server with a provider.

We are agnostic as to which server provider you should choose to run the `xcash-dpops` program. Our recommendation is to find a Dedicated server or VPS that matches the [system requirements](server-setup.md#find-a-linux-machine), and to follow the service you are most confortable with.

{% hint style="info" %}
Make sure you read your server provider documentation. Each initial server setup can be different.
{% endhint %}

## Initialize your server

{% hint style="info" %}
We will give a short tutorial here for preparing a server. Those steps should be easy to replicate with any server hosting providers.
{% endhint %}

### Install a Linux distribution

Your server provider should propose you different Linux distributions to install directly from their dashboard. The **`xcash-dpops`** program was intensively tested on **Ubuntu-18.04 and Ubuntu-20.04** and should be the preferred distribution.

Choose the **Ubuntu 18.04 LTS** or **Ubuntu 20.04 LTS** version and follow the installation process.

### **Log in to your server**

#### **Without SSH key authentification**

**On Windows**

Log into your server using your existing credentials \(given by your server provider\).  
With Putty, indicate the `hostname` \(which should be the server `IP-Address`\) and the port \(the default port is most of the time `port 22`\) open the connection and connect to a user session with your credentials `user`/`password` \(the default user should be `root`, and most times the `password` is sent to you via email by your server provider\).

![](../.gitbook/assets/image%20%2813%29.png)

```text
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-88-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Jun  1 11:51:35 CEST 2020

  System load:    0.02             Processes:             143
  Usage of /home: 0.0% of 4.41TB   Users logged in:       0
  Memory usage:   0%               IP address for enp4s0: 10x.x.x.101
  Swap usage:     0%

68 packages can be updated.
31 updates are security updates.

Last login: Mon Jun  1 11:43:47 2020 from {IP_address}
root@Ubuntu-1804-bionic-64-minimal ~ #
```

**On Linux/Osx**

In a terminal, use the command `ssh` log into your server using your existing credentials \(given by your server provider\). The `hostname` should be the server `IP-Address`, and the default `user` should be `root`.

```text
ssh root@hostname
```

When first connecting to your new server, you will be prompted to recognize the server's host key and validate the RSA fingerprint.

```text
The authenticity of host '{SERVER_IP}' can't be established.
ECDSA key fingerprint is SHA256:AAAAAAAAAAAAfksjlfksjfqmklqjsf+hrQ.
Are you sure you want to continue connecting (yes/no)?
```

Type `yes` and continue the log in process. You will be prompted to provide your password \(which most times, is sent to you via email by your server provider\).

```text
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-88-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Jun  1 11:51:35 CEST 2020

  System load:    0.02             Processes:             143
  Usage of /home: 0.0% of 4.41TB   Users logged in:       0
  Memory usage:   0%               IP address for enp4s0: 10x.x.x.101
  Swap usage:     0%

68 packages can be updated.
31 updates are security updates.

Last login: Mon Jun  1 11:43:47 2020 from {IP_address}
root@Ubuntu-1804-bionic-64-minimal ~ #
```

#### With SSH Authentication

Use your SSH RSA key pair that you have [previously generated](server-setup.md#generate-a-ssh-key) to access your newly rented server. Most of the server hosting service lets you provide your SSH key on their dashboard and automatically authorize it.

**On Windows**

In Putty, go to the `SSH > Auth` tab and browse to your private key file `rsa-key.ppk` [generated earlier](server-setup.md#generate-a-ssh-key) to use as authentication to log into your server.

![](../.gitbook/assets/image%20%2820%29.png)

Now, when you open the connection, you will be prompted to enter your key passphrase \(if you have given one\) to log in.

**On Linux/OSx**

Open a terminal window and use the command `ssh -i`, adding the path to your private key and your servers `user@hostname`

```text
ssh -i ~/.ssh/mykey user@hostname
```

You will be prompted to give your key passphrase if you have provided one at the key generation.

### Manually add your SSH key to your server

{% hint style="info" %}
**In most hosting services, you can register your public SSH key in your server dashboard, which will automatically authorize your SSH key to connect to your server.**  
If you want to add it manually, you can follow the tutorial below. Otherwise, just move on to the next step.
{% endhint %}

With the key pair you have [generated earlier](server-setup.md#generate-a-ssh-key), you will have to register the public key on your server so it can recognize you. To do so, you need to add your public SSH key into the file `~/.ssh/authorized_keys` in your server.

#### From Windows

To copy the content of the public SSH key into the server, you need to create the `authorized_keys` file on your Linux machine.

Once [logged in](server-setup.md#log-in-to-your-server), use your preferred text editor to create and/or open the `authorized_keys` file:

```text
nano ~/.ssh/authorized_keys
```

On your Windows machine, open the public key file.

{% hint style="info" %}
The public key was created after[ generating your SSH key pair](server-setup.md#generate-a-ssh-key) with PuttyGen. You have created two files: the public key `rsa-key` and the private key `rsa-key.ppk`
{% endhint %}

{% code title="rsa-key" %}
```text
---- BEGIN SSH2 PUBLIC KEY ---- 
Comment: "rsa-key-example" ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAl/TYTvi1u/s4JFq0T2A4+jIUKs0jkrqCGfqqFaSClnv4fmexJsmHDW/X84vSYn4tp86NO7h+mxRyEIiI6jilcABOeDOlUtKJuUxg8RJYGL6zbF545IT1jGWg0nR2ZrdRvyFozJgGLJbDd3RzVLnsS3QQvFTRG/mYQu30AIro7jN+KlQX6xVtJBKNuzDelEz5TYuDTkOP9NGcIW78d96L2NPGNM4Qdu3KpjoYecbCUH3D8QjQlvnwEtt/URrHS7nhu2BMRbxN5zP0uT0L1/3NqWnBv367P10kmQFTiBGQo+/nwgxrg6pKUd8AOBkImhpyNR/MJda1UKfZegnxYXb3WQ== 
---- END SSH2 PUBLIC KEY ----
```
{% endcode %}

Copy the public key content \(starting from `ssh-rsa` and the long string after\) and paste it into the `authorized_keys` on your Linux machine.

{% code title="authorized\_keys" %}
```text
ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAl/TYTvi1u/s4JFq0T2A4+jIUKs0jkrqCGfqqFaSClnv4fmexJsmHDW/X84vSYn4tp86NO7h+mxRyEIiI6jilcABOeDOlUtKJuUxg8RJYGL6zbF545IT1jGWg0nR2ZrdRvyFozJgGLJbDd3RzVLnsS3QQvFTRG/mYQu30AIro7jN+KlQX6xVtJBKNuzDelEz5TYuDTkOP9NGcIW78d96L2NPGNM4Qdu3KpjoYecbCUH3D8QjQlvnwEtt/URrHS7nhu2BMRbxN5zP0uT0L1/3NqWnBv367P10kmQFTiBGQo+/nwgxrg6pKUd8AOBkImhpyNR/MJda1UKfZegnxYXb3WQ==
```
{% endcode %}

Save and close the text editor. Now, adjust the permissions of the `authorized_keys` file so that the file does not allow group writable permissions.

```text
chmod 600 ~/.ssh/authorized_keys
```

Once done, restart your server's `ssh.service` before logging out to take your changes into effect:

```text
 sudo systemctl restart ssh.service
```

You should now be able to login to your server using your SSH key. In Putty, browse and choose your private key file `rsa-key.ppk` to use it as an authentication for the next time you log into your server.

![](../.gitbook/assets/image%20%2820%29.png)

Now, when you open the connection, you will be prompted to enter your key passphrase \(if you have given one\), and you will be logged in.

#### From Linux/OSx

You can use the `ssh-copy-id` command to copy the public key directly in the `authorized_keys` on your server.

{% hint style="warning" %}
`ssh-copy-id` is not natively installed on OSx systems. You need to install it by running `brew install ssh-copy-id`
{% endhint %}

Just use the following command by replacing `mykey` with your private key file and `user@host` with your server login information.

```text
ssh-copy-id -i ~/.ssh/mykey user@host
```

If you don't want to use the `ssh_copy_id` command, you can manually append the `authorized_keys` file by using the following command:

```text
cat ~/.ssh/mykey.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

This will take the content of `mykey.pub` and append it to the `authorized_keys` file directly on the server. Once done, restart your server's `ssh.service` before logging out to take your changes into effect:

```text
 sudo systemctl restart ssh.service
```

You can now test the connection to your server by trying to login with your SSH key:

```text
ssh -i ~/.ssh/mykey user@host
```

You will be prompted to give your key passphrase if you have provided one during key generation.

### Create a new user

{% hint style="warning" %}
The **`xcash-dpops`** auto-installer script has been designed for `root` users. It should work for other users as well.
{% endhint %}

[Log in](server-setup.md#log-in) to your server with your already existing credentials \(given by your server provider\) as a `root` user.

Create a new user session named `xcash` where you will only store files and programs related to the delegate function.

```text
sudo adduser xcash
```

Set and confirm the new user’s password at the prompt. It is **highly** recommended to use a password here:

```text
Set password prompts:Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Follow the prompts to set the new user’s information. You can leave it empty.

```text
User information prompts:Changing the user information for xcash
Enter the new value, or press ENTER for the default
    Full Name []:
    Room Number []:
    Work Phone []:
    Home Phone []:
    Other []:
Is the information correct? [Y/n]
```

Now that the `xcash` user is created, you need to give it `sudo` rights:

```text
usermod -aG sudo xcash
```

You can now switch user session from `root` by using the command `su`:

```text
su - xcash
```

## Custom domain name

**Your delegate name is your brand!** ⭐

When you register as a delegate, your server will be recognized and listed in the delegates website through its domain name. If you haven't registered a domain name for your server, the default domain will be the server's IP address.

While in itself it is completely possible to use it as is, it is recommended to brand yourself by buying a domain name of your choice. We believe that it will help you greatly if you are planning to run a shared delegate and are looking for votes.

There are a lot of services out there to buy a domain name: **NameCheap**, **Google Domains**, **OVH**, **GoDaddy,** etc... to only name a few. You will need to create an account over there and reserve the domain name of your choice.

Once you have reserved your domain name, you need to change the DNS record to point your newly bought domain name to your IP address. This can be done by changing the **A \(Address\) Record** in your domain name provider dashboard.

NameCheap has [an easy tutorial](https://www.namecheap.com/support/knowledgebase/article.aspx/319/2237/how-can-i-set-up-an-a-address-record-for-my-domain) for setting up the DNS record, but any other domain name provider should have a similar service.

![Example of a DNS record](../.gitbook/assets/image%20%285%29.png)

Assuming that the domain you bought is **`domain-name.com`**, the A Record above will permit people to identify your server with **`domain-name.com`** and the subdomain **`delegate.domain-name.com`**

## Set up your delegate node

Once you have done every step above, and are familiarized with your server, you can go through the next step and install the delegate program, `xcash-dpops`. Follow the [delegate node installation](https://github.com/X-CASH-official/gitbook-docs/tree/470d72f73fb05e85c916dd3b952cd3fe194fef30/dpops/installation-process.md) for a complete tutorial.

