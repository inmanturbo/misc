```bash
sudo nano /usr/local/bin/microstack-br-workaround
```

```bash
#!/bin/bash
#
# Workaround to enable physical network access to MicroStack
#
# Adds the server physical ip address to br-ex. Replace the physicalcidr and gateway values to match your NIC's IP address and gateway

physicalcidr=$(ip route get 8.8.8.8 | awk -F"src " 'NR==1{split($2,a," ");print a[1]}')
gateway=$(ip route show 0.0.0.0/0 | awk '{print $3}')
# Add IP address to br-ex
ip address add $physicalcidr dev br-ex || :
ip route del default via $gateway || :
ip route add default via $gateway dev br-ex ||:
```
```bash
sudo chmod +x /usr/local/bin/microstack-br-workaround
```

```bash
sudo nano /etc/systemd/system/microstack-br-workaround.service
```

```ini
[Unit]
Description=Service for adding physical ip to microstack bridge
Requires=snap.microstack.external-bridge.service
After=snap.microstack.external-bridge.service

[Service]
ExecStart=/usr/local/bin/microstack-br-workaround
SyslogIdentifier=microstack-br-workaround
Restart=no
WorkingDirectory=/usr/local/bin
TimeoutStopSec=30
Type=oneshot

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl enable microstack-br-workaround.service
```
