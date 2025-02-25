---
layout: single
permalink: /
type: docs
author_profile: false
toc: true
---

<style>
.button {
  border: none;
  color: black;
  #padding: 15px 32px;
  border-radius:4px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}
.button1 {background-color: #4CAF50;} /* Green */
.button2 {background-color: #F70A0A;} /* Red */
.NEW {background-color: #FFF933;} /* yellow */
</style>

Welcome to the Faveo installation documentation.
This documentation shows how to install Faveo Helpdesk on various platforms(OS)

# About Faveo
Faveo is a leading open source, self-hosted, on-premise help desk software. 

[Find out more >>](https://www.faveohelpdesk.com){: .btn .btn--info}

# Installing Faveo <!-- omit in toc -->

Faveo can be installed on a variety of platforms. The choice of the platform is yours.

- [About Faveo](#about-faveo)
- [Requirements](#requirements)
- [Installation instructions for specific platforms](#installation-instructions-for-specific-platforms)
  - [Installation instructions for Freelancer, Paid and Enterprise editions](#installation-instructions-for-freelancer-paid-and-enterprise-editions)
  - [Installation instructions for Community edition](#installation-instructions-for-community-edition)
  - [Installation on a shared server](#installation-on-a-shared-server)
- [Help](#help)
- [One click installers](#one-click-installers)
- [Installation via scripts](#installation-via-scripts)


## How To's <button class="button NEW"><b>NEW</b></button>
- Upgrade to PHP 8.1 [click here](/docs/installation/providers/enterprise/php-upgrade/)

<a id="markdown-requirements" name="requirements"></a>
## Requirements
 

Basically, Faveo depends on the following:

-   **Apache** (with mod_rewrite enabled) or **Nginx** or **IIS**
-   **PHP 7.3.x** with the certain extensions like php-curl, php-gd, php-mbstring, php-pecl-mcrypt, php-mysqlnd, php-odbc, php-pdo etc
-   **Composer**(Optional)
-   **MySQL 8.0.x** or **MariaDB 10.6.x**
-   **Redis** 
-   **Supervisor**

For detailed requirements list [check here](/docs/system-requirement/requirement)


<a id="markdown-installation-instructions-for-specific-platforms" name="installation-instructions-for-specific-platforms"></a>
## Installation instructions for specific platforms

The above mentioned prerequisites to be met have to be installed then faveo can be installed in any OS/Distro from the below list:
<a id="markdown-generic-linux-instructions-enterprise" name="generic-linux-instructions-enterprise"></a>
### Installation instructions for Freelancer, paid and Enterprise editions
* [Cent OS 8 Stream with Apache](/docs/installation/providers/enterprise/centos8s-apache)
* [Cent OS 8 Stream with NGINX](/docs/installation/providers/enterprise/centos8s-nginx)
* [Rocky Linux 8 with Apache](/docs/installation/providers/enterprise/rocky-apache)
* [Rocky Linux 8 with NGINX](/docs/installation/providers/enterprise/centos-nginx)
* [Ubuntu with Apache](/docs/installation/providers/enterprise/ubuntu-apache)<button class="button button1"><b>Recommended</b></button>
* [Ubuntu with NGINX](/docs/installation/providers/enterprise/ubuntu-nginx)
* [Debian with Apache](/docs/installation/providers/enterprise/debian-apache)
* [Debian with NGINX](/docs/installation/providers/enterprise/debian-nginx)
* [Windows](/docs/installation/providers/enterprise/windows)
* [Docker](/docs/installation/providers/enterprise/faveo-helpdesk-docker/)
* [Kubernetes](/docs/installation/providers/enterprise/faveo-helpdesk-k8s/)

<a id="markdown-generic-linux-instructions" name="generic-linux-instructions"></a>
### Installation instructions for Community edition

* [CentOS with Apache](/docs/installation/providers/community/centos-apache)
* [Ubuntu with Apache](/docs/installation/providers/community/ubuntu-apache)
* [Debian with Apache](/docs/installation/providers/community/debian-apache)

<a id="markdown-one-click-installer" name="markdown-one-click-installer"></a>
### One click installers 

Faveo Helpdesk Community can be installed using following installers
- **Softaculous** [https://www.softaculous.com/apps/customersupport/Faveo_Helpdesk](https://www.softaculous.com/apps/customersupport/Faveo_Helpdesk)
- **Fantastico** [https://www.netenberg.com/fantastico.php](https://www.netenberg.com/fantastico.php)


<a id="markdown-script-installer" name="markdown-script-installer"></a>
### Installation via scripts 
Bash script to Auto Installation of Faveo Helpdesk on Linux Operating System. Tested on : RHEL9/8/7, Rocky 9/8, Ubuntu22/20/18, CentOS 9 Stream, Debian 11

  **For Faveo Helpdesk Installation script**
* [Click here](/installation-scripts/FaveoInstallationScripts/faveo-autoscript.sh) to download the "faveo-autoscript.sh" or use wget to download using the below command. 
 
```sh
wget https://github.com/ladybirdweb/faveo-server-images/blob/master/installation-scripts/FaveoInstallationScripts/faveo-autoscript.sh
```

* Once the file is downlaoded to the faveo server provide executable permission to the script.
```
chmod +x faveo-autoscript.sh
```
* After changing the file permission we need to excecute the file by using the below command.
```
./faveo-autoscript.sh
```
* After excecuting the file it will ask for the below values please keep them ready before starting the script.
```
Domain             - (The domain propagated to the faveo server public IP)
Email
Faveo License code - (This can be obtained from https://billing.faveohelpdesk.com)
Faveo Order No     - (This can be obtained from https://billing.faveohelpdesk.com)
```
<b>Note:</b> This script will configured everything requried for Faveo Helpdesk to run that includes configuring supervisor and redis. The supervisor jobs will fail initially until the GUI part of the installation is completed. Run the below command to restart the supervisor daemon.

```sh
systemctl restart supervisor
```
Run the below to check the status of each Jobs
```sh
supervisorctl status
```

<a id="markdown-shared-sever" name="markdown-shared-sever"></a>
### Installation on a shared server <button class="button button2"><b>Not Recommended</b></button>
Faveo can also be installed on a shared server if Emails Incoming/Outgoing is the only requirement, though we highly recommend cloud, VPS or dedicated server for best performance and more control. <br />

<b>Note: </b> In Faveo features like Report generation, Recurring Emails, Notifications etc relies on Redis Database. So if you choose shared hosting like Cpanel. These features will not be available.
* [cPanel](/docs/installation/providers/community/cpanel)

<a id="markdown-help" name="markdown-help"></a>
## Help

If you'd like professional help commercial support is available, email us through the [contact form](https://www.faveohelpdesk.com/contact-us/).
