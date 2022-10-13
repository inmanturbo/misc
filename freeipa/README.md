# FreeIPA NOTES



```bash
sudo hostnamectl set-hostname ipa.mydomain.local
```
### setup static ip and add the hostname to `/etc/hosts`

```bash
sudo firewall-cmd --add-service=freeipa-ldap --add-service=freeipa-ldaps --permanent
```

```bash
sudo dnf install -y freeipa-server freeipa-server-dns nfs-utils
```

```
sudo ipa-server-install --mkhomedir --allow-zone-overlap ##<--needed if domain is already registered elsewhere
```
