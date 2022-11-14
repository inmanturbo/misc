```bash
/usr/bin/wget https://software.virtualmin.com/gpl/scripts/install.sh
```

```bash
/usr/bin/sudo /bin/sh install.sh -b LEMP
```



- Create Dataset for VirtualminMaster i.e `tank/Shares/Unix/VirtualminMaster` 
  - at time of creation, under advanced>acl select nfsv4
- Create NFS share
  - Under advance set maproot user to `root`
  - set maproot group to `root`

### Create User on Truenas system:
```
GID:1001
Home Directory:/mnt/tank/Shares/Unix/VirtualminMaster/virtualm
Shell:/usr/bin/bash
Email:–
Password Disabled:false
Lock User:false
Permit Sudo:false
Microsoft Account:false
Samba Authentication:true
```

- install nfs client
  ```bash
  sudo apt install -y nfs-common
  ```
- prepare mount point
  ```bash
  sudo mkdir -p /home/master
  ```
- add mount point to fstab, e.g:
  ```bash
  echo "truenas.lab.arpa:/mnt/tank/Shares/Unix/VirtualminMaster/virtualm /home/master nfs  rw,async,noatime,hard   0    0" \
    | sudo tee -a /etc/fstab
  ```
 - mount share
   ```bash
   sudo mount -a
   ```
#### Repeat steps above for each top level domain that you wish to store on Truenas!! (Change "master" to top level domain user name)
