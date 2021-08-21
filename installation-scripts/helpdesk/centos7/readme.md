---
layout: single
type: docs
permalink: /installation-scripts/helpdesk/centos7/
redirect_from:
  - /theme-setup/
last_modified_at: 2020-06-09
toc: true
title: "Faveo Helpdesk Freelancer, Enterprise auto install script for CentOS 7"
sidebar:
  nav: "docs"
---

Faveo automatic installation script is available for <b>CentOS 7</b> 

The installation script will install following 
-   **Apache** (with mod_rewrite enabled) 
-   **PHP 7.3+** with the following extensions: curl, dom, gd, json, mbstring, openssl, pdo_mysql, tokenizer, zip
-   **MySQL 5.7+** 

This script should be used only on fresh/new copy of CentOS 7

## Run the script

To run, copy/paste this into the command-line
    
```sh
yum -y install wget
wget https://github.com/ladybirdweb/faveo-server-images/blob/master/installation-scripts/helpdesk/centos7/autoinstall.sh
```

Change execution permission for file.

```sh
chmod +x autoinstall.sh
```

Execute the script

```sh
./autoinstall.sh
```

## Install Faveo

Now you can install Faveo via [GUI](/docs/installation/installer/gui) Wizard or [CLI](/docs/installation/installer/cli)