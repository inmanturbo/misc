# Graphical workstation kvm lab

## First install ubuntu deskop with zfs on root
- #### Get the desktop iso
  ```bash
  curl https://releases.ubuntu.com/22.04/ubuntu-22.04.1-desktop-amd64.iso -o ubuntu-22.04.1-desktop-amd64.iso
  ```
- #### flash USB using balena or dd (dd example below)
  > This could destroy your system if you flash to the wrong drive! Identify your flash drive first.
  > The hash at the start of the command here is for safety purposes if your are copy/pasting.
  > You must fist edit the command and remove the hash in order to run it!
  ```
  # sudo dd if=ubuntu-22.04.1-desktop-amd64.iso of=/dev/sdx bs=1024k status=progress
  ```

## Install Packages and setup shell
```bash
sudo apt install -y qemu-kvm virt-manager libvirt-daemon-system virtinst libvirt-clients \
  bridge-utils zsys cockpit cockpit-machines net-tools \
  zsh software-properties-common curl git libnss3-tools jq xsel xrdp
```
```bash
echo "${USER} ALL=(ALL) NOPASSWD: ALL" |sudo tee -a /etc/sudoers
```
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions \
  && git clone https://github.com/larkery/zsh-histdb $HOME/.oh-my-zsh/custom/plugins/zsh-histdb \
  && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting \
  && sed -i '/plugins=(git)/c\plugins=(git zsh-autosuggestions zsh-syntax-highlighting)' ~/.zshrc \
  && source ~/.zshrc
```
```bash
sudo nano /etc/gdm3/custom.conf
```
uncomment `WaylandEnabled=false`

```bash
sudo nano /etc/default/grub
```
```
GRUB_CMDLINE_LINUX_DEFAULT="intel_iommu=on"
```
```bash
sudo update-grub
```
### Required Modules
add to `etc/modules`

```bash
sudo nano /etc/modules
```
```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

### Enable sata controller to be passed through to TrueNAS VM

```bash
lspci |grep SATA
```
```
07:00.2 SATA controller: Advanced Micro Devices, Inc. [AMD] FCH SATA Controller [AHCI mode] (rev 51)
```
```
echo "options vfio-pci ids=07:00.2" | sudo tee -a /etc/modprobe.d/vfio.conf
```
### Reboot
``` bash
sudo reboot now
```
### Check for iommu
```
sudo dmesg | grep -e DMAR -e IOMMU
```

#### Finally, go to settings>power and set `Screen Blank` to never, and `Automatic Suspend` to Off

