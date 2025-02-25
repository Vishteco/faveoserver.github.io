---
layout: single
type: docs
permalink: /docs/installation/providers/enterprise/debian-nginx-ssl/
redirect_from:
  - /theme-setup/
last_modified_at: 2020-06-09
toc: true
---

# Install Let’s Encrypt SSL for Faveo on Debian Running Nginx Web Server <!-- omit in toc -->

<img alt="debian" src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4a/Debian-OpenLogo.svg/109px-Debian-OpenLogo.svg.png" width="120" height="120" />

## Introduction
This document will list on how to install LetsEncrypt SSL on Debian Running Nginx Web Server

PS : Please replace example.com with your valid domain name which is mapped with your server

We will install following dependencies in order to make LetsEncrypt SSL work:

- python-certbot-nginx


## Downloading the LetsEncrypt client for Debian 10 (Buster)

```sh
apt install -y python-certbot-nginx
```
## Setting up the SSL certificate

Certbot will handle the SSL certificate management quite easily, it will generate a new certificate for provided domain as a parameter.

In this case, example.com will be used as the domain for which the certificate will be issued:

```sh
certbot --nginx -d example.com
```
If you want to generate SSL for multiple domains or subdomains, please run this command:

```sh
certbot --nginx -d example.com -d www.example.com
```
**PS :** IMPORTANT! The first domain should be your base domain, in this sample it’s example.com

## Setting up auto renewal of the certificate

Create new cron job for automatic renewal of SSL

This job can be safely scheduled to run every Monday at midnight:

Create a new `/etc/cron.d/faveo-ssl` file with:

```sh
echo "45 2 * * 6 /etc/letsencrypt/ && ./certbot renew && /etc/init.d/nginx restart " | sudo tee /etc/cron.d/faveo-ssl
```