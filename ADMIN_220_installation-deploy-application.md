---
id: admin-installation-deploy-application
title: Deploy application
sidebar_label: Deploy application
---

## Directory structure
Create directory structure with following commands:
```
$ mkdir agile-factory
$ mkdir agile-factory/_releases         # Store releases here, to easily rollback to a previous version
$ mkdir agile-factory/bin               # Binaries
$ mkdir agile-factory/logs              # Logs
$ mkdir agile-factory/storage           # S3 storage (docs and assets)
$ mkdir agile-factory/www
$ mkdir agile-factory/www/docs          # Documentation
$ mkdir agile-factory/www/hmi           # HMI interface
$ mkdir agile-factory/www/manager       # Manager interface
$ mkdir agile-factory/www/simulator     # Machine simulator
```

# Deploy application
```
$ cd agile-factory/storage
$ tar xfj $HOME/agile-factory/_releases/assets.tar.bz2
$ cd agile-factory/bin
$ tar xfj $HOME/agile-factory/_releases/engine_release.tar.bz2
$ cd agile-factory/www/docs
$ tar xfj $HOME/agile-factory/_releases/docs_release.tar.bz2
$ cd agile-factory/www/hmi
$ tar xfj $HOME/agile-factory/_releases/hmi_release.tar.bz2
$ cd agile-factory/www/manager
$ tar xfj $HOME/agile-factory/_releases/manager_release.tar.bz2
$ cd agile-factory/www/simulator
$ tar xfj $HOME/agile-factory/_releases/simulator_release.tar.bz2
```
