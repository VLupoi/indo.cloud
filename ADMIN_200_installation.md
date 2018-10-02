---
id: admin-installation
title: Install guide
sidebar_label: Install guide
---

**Note** install guide is made for Ubuntu 16.04 LTS.

## Before installation
Please update packages using apt

## Installation
Copy `installer.run` provided by Agile Factory on server and execute it as root:

```
$ sudo ./installer.run
```

Accept disclaimer and insert License number, then wait for install 


We assume that:
* the application user will be `user`;
* the home directory will be `/home/user`;
* the used will be `domain.tld`.

## Checklist

* [Required software](admin-installation-required-software.html)
    * [MongoDB](admin-installation-required-software.html#mongodb)
    * [Nginx](admin-installation-required-software.html#nginx)
    * [AF-Engine](admin-installation-required-software.html#af-engine)
    * [AF-Storage](admin-installation-required-software.html#af-storage)
    * [Mqtt broker](admin-installation-required-software.html#mqtt-broker)
* [Deploy application](admin-installation-deploy-application.html)
