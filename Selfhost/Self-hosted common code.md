
# Init and always restart service
```bash
nano /etc/systemd/system/docker-compose-app.service
```

```bash
sudo systemctl enable docker-compose-app.service
```

```bash
sudo systemctl start docker-compose-app.service
```

```bash
systemctl status docker-compose-app.service
```

```bash
[Unit]
Description=Docker Compose Application Service
Requires=docker.service
After=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/root/java_spring_udemy_coupon
ExecStart=/usr/bin/docker compose up -d
ExecStop=/usr/bin/docker compose down
TimeoutStartSec=0

[Install]
WantedBy=multi-user.target
```



iptables -t nat -A PREROUTING -i vmbr0 -p udp --dport 1194 -j DNAT --to-destination 192.168.1.15:1194 iptables -A FORWARD -p udp -d 192.168.1.15 --dport 1194 -j ACCEPT