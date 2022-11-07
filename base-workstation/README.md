```bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install -y openssh-server
ip a
echo "inman ALL=(ALL) NOPASSWD: ALL" |sudo tee -a /etc/sudoers
sudo apt install zsh software-properties-common curl git network-manager libnss3-tools jq xsel
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions \
  && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting \
  && sed -i '/plugins=(git)/c\plugins=(git zsh-autosuggestions zsh-syntax-highlighting)' ~/.zshrc \
  && source ~/.zshrc
 ```
