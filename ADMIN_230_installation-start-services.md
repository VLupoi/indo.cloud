---
id: admin-installation-start-services
title: Start services
sidebar_label: Start services
---

## Nginx
```
$ sudo systemctl restart nginx
$ sudo systemctl status nginx
```

## Storage
```
$ sudo systemctl start af-storage.service
$ sudo systemctl status af-storage.service
```


## Mqtt broker
```
$ sudo systemctl start vernemq.service
$ sudo systemctl status vernemq.service
```

## Agile Factory Engine
```
$ sudo systemctl start af-engine
$ sudo systemctl status af-engine
```

If anyone of these services is down, the application will not work.

Please check service log, and if you cannot fix the problem contact the [support](admin-support.html)