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
: 1665598087:0;sudo reboot now
: 1665598238:0;sudo apt install -y cockpit-machines
: 1665599179:0;sudo nano /etc/sshd
: 1665599189:0;sudo nano /etc/ssh/sshd_config
: 1665599218:0;sudo systemctl restart sshd
: 1665599221:0;sudo systemctl restart ssh
: 1665599223:0;exit
: 1665599398:0;sudo sudo usermod -aG libvirt-qemu $USER
: 1665599440:0;sudo usermod -aG libvirt-qemu $USER
: 1665599444:0;exit
: 1665599447:0;groups
: 1665599492:0;sudo chown -R libvirt-qemu:kvm /home/inman/Downloads
: 1665599499:0;ls -l
: 1665599513:0;sudo systemctl restart libvirtd
: 1665600332:0;cd Downloads
: 1665600821:0;clear
: 1665600825:0;lsblk
: 1665600831:0;clear
: 1665600842:0;lspci
: 1665600850:0;clear
: 1665600856:0;lspci |more
: 1665601591:0;sudo nano nano /etc/default/grub
: 1665601610:0;sudo nano /etc/default/grub
: 1665601700:0;sudo update-grub
: 1665601742:0;sudo nano  /etc/modules
: 1665601775:0;dmesg | grep -e DMAR -e IOMMU
: 1665601782:0;sudo dmesg | grep -e DMAR -e IOMMU
: 1665601799:0;sudo reboot now
: 1665601913:0;sudo dmesg | grep -e DMAR -e IOMMU
: 1665601930:0;virt-manager &
: 1665602299:0;sudo nano  /etc/modules
: 1665602341:0;lspci |more
: 1665602457:0;echo "options vfio-pci ids=06:00.2" | sudo tee -a /etc/modprobe.d/vfio.conf
: 1665602504:0;sudo reboot now
: 1665602919:0;sudo nano /etc/default/grub
: 1665602941:0;sudo update-grub
: 1665602951:0;sudo reboot now
: 1665603055:0;sudo dmesg | grep -e DMAR -e IOMMU
```
