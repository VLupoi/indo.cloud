---
id: admin-test-services
title: Test Services
sidebar_label: Services
---

**Note** If you receive any error please contact [support](admin-support.html).

## Checklist

* Mqtt broker
* MongoDB
* AF-Storage
* AF-Engine
* Nginx and vhosts

## Check daemons
Open an ssh session to the server and do the following checks.

### Mqtt Broker
Check that service is up

```
$ sudo systemctl status vernemq
```

The output will be the following
```
● vernemq.service - VerneMQ is a MQTT message broker
   Loaded: loaded (/lib/systemd/system/vernemq.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2018-02-13 13:38:10 CET; 1 weeks 0 days ago
 Main PID: 28361 (beam.smp)
    Tasks: 163
   Memory: 114.1M
      CPU: 1h 5min 11.341s
   CGroup: /system.slice/vernemq.service
           ├─28350 /usr/lib/vernemq/erts-8.3.5.3/bin/epmd -daemon
           ├─28358 /usr/lib/vernemq/erts-8.3.5.3/bin/run_erl -daemon /tmp/vernemq// /var/log/vernemq exec /usr/sbin/vernemq console
           ├─28361 /usr/lib/vernemq/erts-8.3.5.3/bin/beam.smp -P 256000 -A 64 -K true -W w -- -root /usr/lib/vernemq -progname vernemq -- -home /var/lib/vernemq -- -boo
           ├─28573 erl_child_setup 1024
           ├─28579 sh -s disksup
           ├─28581 /usr/lib/vernemq/lib/os_mon-2.4.2/priv/bin/memsup
           └─28582 /usr/lib/vernemq/lib/os_mon-2.4.2/priv/bin/cpu_sup

Feb 20 15:27:28 ic-www-01 systemd[1]: vernemq.service: Supervising process 28361 which is not our child. We'll most likely not notice when it exits.
Feb 20 15:27:28 ic-www-01 systemd[1]: vernemq.service: Supervising process 28361 which is not our child. We'll most likely not notice when it exits.
```

To check if mqtt broker is working try to see verify that listening port is opened and listening on 0.0.0.0.
```
$  sudo netstat -punta |grep 1883
tcp        0      0 0.0.0.0:1883            0.0.0.0:*               LISTEN      28361/beam.smp
```

Install mosquitto-client
```
$ sudo apt install mosquitto-clients
```

Subcribe to a test topic
```
$ mosquitto_sub -h localhost -p 1883 -t test.topic
```

On a different ssh session publish a test message
```
$ mosquitto_pub -h localhost -p 1883 -t test.topic -m "Test message"
```

On the first session you should see the message
```
$ mosquitto_sub -h localhost -p 1883 -t test.topic
Test message
```

### MongoDB
Check that service is up
```
$ sudo systemctl status mongod
```
```
● mongod.service - High-performance, schema-free document-oriented database
   Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vendor preset: enabled)
   Active: active (running) since Tue 2018-02-13 13:40:43 CET; 1 weeks 0 days ago
     Docs: https://docs.mongodb.org/manual
 Main PID: 28959 (mongod)
    Tasks: 31
   Memory: 641.5M
      CPU: 1h 33min 6.343s
   CGroup: /system.slice/mongod.service
           └─28959 /usr/bin/mongod --config /etc/mongod.conf

Feb 20 15:46:04 ic-www-01 systemd[1]: Started High-performance, schema-free document-oriented database.
```

Connect to mongo instance and check that there is data inside database:

```
$ mongo
MongoDB shell version v3.6.2-rc0
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.2-rc0
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user
> use agile-factory
switched to db agile-factory
> db.factories.count();
1
> db.areas.count();
1
> db.machines.count();
6
> db.users.count()
6
> db.phase_types.count();
28
>
```

If any queries returns 0 documents there is a problem, the software will not work properly.

### AF-Storage
Check that service is up

```
$ sudo systemctl status af-storage
```

The output will be the following
```
* af-storage.service - Agile Factory storage
   Loaded: loaded (/lib/systemd/system/af-storage.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2018-02-15 14:11:41 CET; 5 days ago
 Main PID: 16144 (minio)
    Tasks: 9
   Memory: 6.3M
      CPU: 0
   CGroup: /system.slice/af-storage.service
           `-16144 /home/user/go/bin/minio server --address localhost:9001 /home/user/agile-factory/storage/
```

Check that the daemon is listening on port 9001
```
$ sudo netstat -punta |grep 9001
tcp        0      0 127.0.0.1:9001          0.0.0.0:*               LISTEN      16144/minio
```

### AF-Engine
Check that service is up

```
$ sudo systemctl status af-engine
```

The output will be the following
```
* af-engine.service - Agile Factory Engine
   Loaded: loaded (/lib/systemd/system/af-engine.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2018-02-20 16:01:58 CET; 5s ago
 Main PID: 2132 (af-api)
    Tasks: 9
   Memory: 120.1M
      CPU: 0
   CGroup: /system.slice/af-engine.service
           `-2132 /home/user/agile-factory/bin/af-api
```
Check that the daemon is listening on port 9000
```
$ sudo netstat -punta |grep 9000
tcp6       0      0 :::9000                 :::*                    LISTEN      2132/af-api
```


### Nginx
Check that service is up
```
$ sudo systemctl status nginx
```

The output will be the following
```
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2018-02-19 12:17:20 CET; 1 day 3h ago
 Main PID: 21221 (nginx)
    Tasks: 3
   Memory: 19.5M
      CPU: 56.748s
   CGroup: /system.slice/nginx.service
           ├─21221 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
           ├─21222 nginx: worker process
           └─21223 nginx: worker process
```

#### Test websites

##### API
Open `http://api.domain.tld`, you should read the following message:

```
Agile Factory up and running
````

##### Storage
Open `http://s3.domain.tld` and insert the credentials used during S3 configuration [see here](admin-installation-required-software.html#af-storage).

##### HMI interface
Open `http://hmi.domain.tld` and be sure that there is no error.

##### Manager interface
Open `http://manager.domain.tld` and be sure that there is no error, then try to sign in to the manager interface using default credentials:

**Email** `administrator@agilefactory.it`
**Password** `agile`