```zsh
: 1665596903:0;sudo apt install -y cockpit
: 1665597215:0;sudo apt install -y qemu-kvm virt-manager libvirt-daemon-system virtinst libvirt-clients bridge-utils
: 1665597337:0;zsysctl
: 1665597433:0;sudo apt install -y zsys
: 1665597468:0;sudo apt install -y net-tools
: 1665597722:0;sudo apt autoremove
: 1665597758:0;sudo usermod -aG kvm $USER
: 1665597765:0;sudo usermod -aG libvirt $USER
: 1665597932:0;sudo apt update && sudo apt upgrade -y
: 1665598238:0;sudo apt install -y cockpit-machines
: 1665599179:0;sudo nano /etc/sshd
: 1665599189:0;sudo nano /etc/ssh/sshd_config
: 1665599218:0;sudo systemctl restart sshd
: 1665599221:0;sudo systemctl restart ssh
: 1665599398:0;sudo sudo usermod -aG libvirt-qemu $USER
: 1665599440:0;sudo usermod -aG libvirt-qemu $USER
: 1665599492:0;sudo chown -R libvirt-qemu:kvm /home/inman/Downloads
: 1665599513:0;sudo systemctl restart libvirtd
: 1665600856:0;lspci |more
: 1665601591:0;sudo nano nano /etc/default/grub
: 1665601610:0;sudo nano /etc/default/grub
: 1665601700:0;sudo update-grub
: 1665601742:0;sudo nano  /etc/modules
: 1665601775:0;dmesg | grep -e DMAR -e IOMMU
: 1665601782:0;sudo dmesg | grep -e DMAR -e IOMMU
: 1665601799:0;sudo reboot now
: 1665601913:0;sudo dmesg | grep -e DMAR -e IOMMU
: 1665602299:0;sudo nano  /etc/modules
: 1665602341:0;lspci |more
: 1665602457:0;echo "options vfio-pci ids=06:00.2" | sudo tee -a /etc/modprobe.d/vfio.conf
: 1665602504:0;sudo reboot now
: 1665602919:0;sudo nano /etc/default/grub
: 1665602941:0;sudo update-grub
: 1665602951:0;sudo reboot now
: 1665603055:0;sudo dmesg | grep -e DMAR -e IOMMU
```

```bash
sudo apt install -y qemu-kvm virt-manager libvirt-daemon-system virtinst libvirt-clients bridge-utils zsys cockpit cockpit-machines net-tools \
  zsh software-properties-common curl git libnss3-tools jq xsel
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

