## macOS setup notes

### Install Homebrew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew --version
```
### Software
```bash
brew install go 
brew install kubectl
brew install htop
brew install --cask powershell
```
### To use Homebrew in PowerShell run: 
`Add-Content -Path $PROFILE.CurrentUserAllHosts -Value '$(/usr/local/bin/brew shellenv) | Invoke-Expression'`
```bash
brew install terraform
brew install python3
brew install python-tk@3.9
brew install jq
brew install graphicsmagick 
```
### Other brew commands
```bash
rm '/usr/local/bin/terraform'
brew link --overwrite terraform
brew link terraform

brew doctor
brew cleanup
```
### [VScode](https://code.visualstudio.com/sha/download?build=stable&os=darwin-universal)
```bash
brew install vscode
```
### Install developer tools
`xcode-select --install`

### Openssl
```bash
brew install openssl
export LDFLAGS="-L/usr/local/opt/openssl@1.1/lib"
export CPPFLAGS="-I/usr/local/opt/openssl@1.1/include"
echo 'export PATH="/usr/local/sbin:$PATH"' >> ~/.zshrc
```   
### az cli
```bash
echo $PATH
brew update && brew install azure-cli
brew update && brew upgrade azure-cli
```  
### aws sam cli
```
brew tap aws/tap
brew install aws-sam-cli
sam --version
brew install python@3.8
brew upgrade aws-sam-cli
echo 'export PATH="/usr/local/opt/python@3.8/bin:$PATH"' >> ~/.zshrc
export LDFLAGS="-L/usr/local/opt/python@3.8/lib"
export PKG_CONFIG_PATH="/usr/local/opt/python@3.8/lib/pkgconfig"
```
### pyenv    
`brew install pyenv`
    
### enable tab completion
`terraform -install-autocomplete`

### Terraform commands
`terraform fmt`
`terraform show`
`terraform apply`

### Consul
https://releases.hashicorp.com/consul/1.9.4/consul_1.9.4_darwin_amd64.zip
    
### [Postman api](https://dl.pstmn.io/download/latest/osx)

https://www.npmjs.com/package/newman 

`brew install newman`
        
https://www.npmjs.com/package/newman#command-line-options
        
https://www.npmjs.com/package/newman

### ssh cert for git
```bash
ssh-keygen -t ed25519 -C "email_address"
eval "$(ssh-agent -s)"
touch ~/.ssh/config
open ~/.ssh/config
Host *
AddKeysToAgent yes
UseKeychain yes
IdentityFile ~/.ssh/id_ed25519    
ssh-add -K ~/.ssh/id_ed25519
```       
[git api](https://docs.github.com/en/free-pro-team@latest/rest/reference)

### get public ip
```
my_ip=$(dig +short myip.opendns.com @resolver1.opendns.com);
echo $my_ip
```
### Disable SafeSleep Hibernation Mode
```bash
sudo pmset -a hibernatemode 0
cd /private/var/vm/
sudo rm sleepimage
sudo touch sleepimage
sudo chmod 000 /private/var/vm/sleepimage
```
### Enable SafeSleep
`sudo pmset -a hibernatemode 3; sudo rm /private/var/vm/sleepimage`

### Remove Speech Voices (~876M	Voices)
```bash
cd /System/Library/Speech/
sudo rm -rf Voices/*
```
### Clear logs
`sudo rm -rf /private/var/log/*`

### User cache files
`cd ~/Library/Caches/`
