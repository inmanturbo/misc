
# Samba domain controller

### Original unhappy path

```bash
echo "$USER ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers
sudo hostnamectl set-hostname ubuntu-base
sudo apt update
sudo apt upgrade -y
exti
exit
sudo hostnamectl set-hostname dc1
sudo nano /etc/netplan/00-installer-config.yaml
sudo netplan apply
ip a
sudo reboot now
sudo nano /etc/hosts
sudo nano /etc/hostname
exit
sudo apt install -y samba windbind krb5-config smbclient dnsutils net-tools
sudo apt install -y samba winbind krb5-config smbclient dnsutils net-tools
sudo  mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
sudo samba-tool domain provision --use-rfc2307 --interactive
sudo cp /var/lib/samba/private/krb5.conf /etc
sudo nano /etc/resolv.conf
sudo apt install resolvconf
sudo nano /etc/resolvconf/resolv.conf.d/head
y
sudo nano /etc/resolv.conf
sudo systemctl disable --now resolvconf
sudo apt remove resolvconf
sudo nano /etc/resolv.conf
sudo systemctl disable --now smbd nmbd winbind systemd-resolved.service
sudo nano /etc/resolv.conf
sudo systemctl unmask samba-ad-dc.service
sudo systemctl enable --now samba-ad-dc.service
netstat -antp | egrep 'smdb|samba'
sudo netstat -antp | egrep 'smdb|samba'
sudo netstat -antp | egrep 'smdb'
sudo netstat -antp | egrep 'smbd'
sudo netstat -antp | egrep 'smbd|samba'
ping google.com
cat $HISTORY
cat $HISTFILE
exit
clear
ip 
ip a
sudo nano /etc/resolv.conf
sudo nano /etc/resolve.conf
sudo nano /etc/resolv.conf
exit
cat $HISTFILE
```

### Refinements

### install cockpit for easy interface configuration

```
sudo apt install -y cockpit
```

Install NetworkManager on Ubuntu:
```bash
sudo apt-get install network-manager
```

Open the .yaml config file inside the /etc/netplan directory and replace the existing configuration with following:
```yaml
network:
  version: 2
  renderer: NetworkManager
  ```
  
Generate backend specific configuration files for NetworkManager with netplan command:

```bash
sudo netplan generate
```
Start the NetworkManager Service:

```bash
sudo systemctl unmask NetworkManager
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
```
Now the NetworkManager is enabled, interface configurations can be done via the GUI or from the command line, using the nmcli command.
