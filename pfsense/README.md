### Rest api for pfsense
#### Installation
```bash
pkg add https://github.com/jaredhendrickson13/pfsense-api/releases/latest/download/pfSense-2.6-pkg-API.txz && /etc/rc.restart_webgui
```

New DNS Entry Example using curl (`host override`)
```bash
curl -u "admin:${PASSWORD}" -X POST http://pfsense/api/v1/services/unbound/host_override \
  -H 'Content-Type: application/json' \
  -d '{"domain":"example.com","host":"auth", "ip":"192.168.222.99"}'
 ```

### Adding mx records to unbound using custom options
Put this at the top:
```
server:qname-minimisation: yes
```

mx records:
```
local-data: "mail.example.com. IN MX 10 mail.example.com."
local-data: "*.example.com. IN MX 10 mail.example.com."
```
