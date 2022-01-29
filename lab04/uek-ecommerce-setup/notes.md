## UEK ecommerce automatic setup

### TODO

1. install java
2. download release v1.1
3. start app

### TODO2

1. Setup as System Service
2. App own user
3. App own dir
4. App own config
5. Start with system
6. sudo systemctl status uekecommerce
7. sudo systemctl start/stop/restart uekecommerce

* KONWENCJA

### TODO3 Skalowanie

Nginx as load-balancer / reverse proxy


### 4

`system/uek-ecommerce.service`

```
[Unit]
Description=My nice service
After=online.target

[Service]
Type=simple
User=uekr-ecommerce
ExecStart=java -jar /opt/uek-ecommerce/usek-ecommerce.jar
Restart=always

[Install]
WantedBy=multi_user.target
```