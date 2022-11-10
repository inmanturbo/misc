- Create Dataset for Unix shares, i.e `tank/Shares/Unix`
- Create Dataset for docker i.e `tank/Shares/Unix/Docker` 
  - at time of creation, under advanced>acl select nfsv4
- Create user in in truenas scale
  - any name uid of `1000`
- Set permissions
  - Select View Permissions>Edit Permissions for `tank/Shares/Unix/Docker`
  - set user to user created above
  - set group to user created above
- Create NFS share
  - Under advance set maproot user to `root`
  - set maproot group to `root`

- Create an ubuntu VM with a single sudo capable user
- install Docker
  ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh \
   && sudo sh get-docker.sh
   ```
- install docker-compose
```bash
sudo apt install -y docker-compose
```
- add user to docker group
```bash 
sudo usermod -aG docker $USER
```
```bash
newgrp docker
```
test installation
```bash
docker run hello-world
```
