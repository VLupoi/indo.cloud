---
id: admin-installation-required-software
title: Required software
sidebar_label: Required software
---

## MongoDB
Installation taken from the following [guide](https://docs.mongodb.com/master/tutorial/install-mongodb-on-ubuntu/)
```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
$ sudo echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
$ sudo apt update
$ sudo apt install -y mongodb-org
$ sudo systemctl enable mongod
$ sudo systemctl start mongod
$ sudo systemctl status mongod
```

## AF-Engine
Create the following file `/lib/systemd/system/af-engine.service`:
```
[Unit]
Description=Agile Factory Engine
After=network.target

[Service]
User=agile
Environment=_____________________________________________________________________________________________CORE___________________________________________________________=
Environment=NODE_ENV=production
Environment=DEBUG=af:*
Environment=BABEL_DISABLE_CACHE=1
Environment=JWT_SECRET=[WRITE_A_JWT_SECRET]
Environment=SERIAL_NO=[WRITE_SERIAL]
Environment=SUBSCRIPTIONS_ENDPOINT=ws://api.domain.tld/subscriptions
Environment=_____________________________________________________________________________________________CORE_FILE_STORAGE______________________________________________=
Environment=MINIO_ACCESS_KEY=[WRITE_AN_ACCESS_KEY]
Environment=MINIO_SECRET_KEY=[WRITE_A_SECRET_KEY]
Environment=FILE_STORAGE=http://s3.domain.tld
Environment=_____________________________________________________________________________________________CORE_MQTT______________________________________________________=
Environment=MQTT_SERVER=mqtt://localhost
ExecStart=/home/agile/agile-factory/bin/af-engine

[Install]
WantedBy=multi-user.target
```

## Enable AF-Engine service
```
$ sudo systemctl enable af-engine
```

## AF-Storage
Execute following commands:
```
$ mkdir -p $HOME/agile-factory/storage $HOME/agile-factory/bin
$ wget https://dl.minio.io/server/minio/release/linux-amd64/minio -O $HOME/agile-factory/bin/minio
$ chmod +x $HOME/agile-factory/bin/minio
```

Create file `/lib/systemd/system/af-storage.service` with the following content:
```
[Unit]
Description=Minio S3 object storage
After=network.target

[Service]
User=agile
Environment=MINIO_ACCESS_KEY=[WRITE_AN_ACCESS_KEY]
Environment=MINIO_SECRET_KEY=[WRITE_A_SECRET_KEY]
ExecStart=/home/agile/agile-factory/bin/minio server --address localhost:9001 /home/agile/agile-factory/storage

[Install]
WantedBy=multi-user.target
```

Enable service:
```
$ sudo systemctl enable af-storage.service
```

Minio will listen on port 9001.

## Mqtt broker
Download latest version from `https://vernemq.com/downloads/index.html`
```
# wget https://bintray.com/artifact/download/erlio/vernemq/deb/xenial/vernemq_1.4.1-1_amd64.deb
# dpkg -i vernemq_1.4.1-1_amd64.deb
```
Modify file `/etc/vernemq/vernemq.conf`
```
allow_anonymous = on
listener.tcp.default = 0.0.0.0:1883
```

Update Broker service configuration `/lib/systemd/system/vernemq.service`
```
[Unit]
Description=VerneMQ is a MQTT message broker

[Service]
ExecStart=/usr/sbin/vernemq start
ExecStop=/usr/sbin/vernemq stop
User=vernemq
Type=forking
LimitNOFILE=65536
PIDFile=/var/run/vernemq/vernemq.pid
EnvironmentFile=-/etc/default/vernemq
RuntimeDirectory=vernemq

[Install]
WantedBy=multi-user.target
```

```
$ sudo systemctl daemon-reload
```


## Nginx
```
$ sudo apt-get install nginx
```
Add following lines inside `/etc/nginx/nginx.conf`

```
server_names_hash_bucket_size 128;
client_max_body_size 16M;
upstream af-engine {
    server localhost:9000;
}
upstream af-storage {
    server localhost:9001;
}
```

### AF-Storage
 
Create the following file `/etc/nginx/sites-available/s3.domain.tld.conf`
```
server {
    server_tokens off;
    listen 80;
    server_name s3.domain.tld;


    location / {
        proxy_pass http://af-storage;

        gzip                    off;
    
        proxy_read_timeout      300;
        proxy_connect_timeout   300;
        proxy_redirect          off;
    
        proxy_set_header        Host                $http_host;
        proxy_set_header        X-Real-IP           $remote_addr;
        proxy_set_header        X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header        X-Forwarded-Proto   https;
        proxy_set_header        X-Frame-Options     SAMEORIGIN;
    }
}
```

Enable virtual host
```
$ sudo ln -s /etc/nginx/sites-available/s3.domain.tld.conf /etc/nginx/sites-enabled
```

### AF-Engine

Create the following file `/etc/nginx/sites-available/api.domain.tld.conf`

```
server {
    client_max_body_size 16M;
    server_tokens off;
    listen 80;
    server_name api.domain.tld;
    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://af-engine;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

Enable virtual host
```
$ sudo ln -s /etc/nginx/sites-available/api.domain.tld.conf /etc/nginx/sites-enabled
```



### Documentation

Create the following file `/etc/nginx/sites-available/docs.domain.tld.conf`
```
server {
    listen              80;
    server_tokens off;
    root                /home/agile/agile-factory/www/docs;
    access_log          /home/agile/agile-factory/logs/docs-access.log;
    error_log           /home/agile/agile-factory/logs/docs-error.log;
    server_name         docs.domain.tld;
    location / {
        try_files $uri /index.html;
    }
}
```

Enable virtual host
```
$ sudo ln -s /etc/nginx/sites-available/hmi.domain.tld.conf /etc/nginx/sites-enabled
```

### HMI

Create the following file `/etc/nginx/sites-available/hmi.domain.tld.conf`
```
server {
    listen              80;
    server_tokens off;
    root                /home/agile/agile-factory/www/hmi;
    access_log          /home/agile/agile-factory/logs/hmi-access.log;
    error_log           /home/agile/agile-factory/logs/hmi-error.log;
    server_name         ~^(.*)\.domain\.tld;
    location / {
        try_files $uri /index.html;
    }
}
```

Enable virtual host
```
$ sudo ln -s /etc/nginx/sites-available/hmi.domain.tld.conf /etc/nginx/sites-enabled
```

### Manager

Create the following file `/etc/nginx/sites-available/manager.domain.tld.conf`
```
server {
    client_max_body_size 16M;
    listen              80;
    server_tokens off;
    root                /home/agile/agile-factory/www/manager;
    access_log          /home/agile/agile-factory/logs/manager-access.log;
    error_log           /home/agile/agile-factory/logs/manager-error.log;
    server_name         manager.domain.tld;
    location / {
        try_files $uri /index.html;
    }
}
```
```
$ sudo ln -s /etc/nginx/sites-available/manager.domain.tld.conf /etc/nginx/sites-enabled
```

