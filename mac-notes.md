## macOS setup notes

### Install Homebrew
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew --version
```
### Other brew commands
```shell
brew doctor
brew cleanup
```
### VScode
```
https://code.visualstudio.com/sha/download?build=stable&os=darwin-universal
```
### Terraform
```shell
curl -O https://releases.hashicorp.com/terraform/0.15.5/terraform_0.15.5_darwin_amd64.zip
unzip terraform_0.15.5_darwin_amd64.zip
mv terraform /usr/local/bin/
rm terraform_0.15.5_darwin_amd64.zip
```
### Install developer tools
`xcode-select --install`

### Openssl
```shell
brew install openssl
export LDFLAGS="-L/usr/local/opt/openssl@1.1/lib"
export CPPFLAGS="-I/usr/local/opt/openssl@1.1/include"
echo 'export PATH="/usr/local/sbin:$PATH"' >> ~/.zshrc
```
### Magic packet
`brew install wakeonlan`
    
### az cli
```shell   
echo $PATH
brew update && brew install azure-cli
brew update && brew upgrade azure-cli
```  

az upgrade
    
### aws sam cli
    brew tap aws/tap
    brew install aws-sam-cli
    sam --version
    
    brew install python@3.8
    brew upgrade aws-sam-cli
    
    echo 'export PATH="/usr/local/opt/python@3.8/bin:$PATH"' >> ~/.zshrc
    export LDFLAGS="-L/usr/local/opt/python@3.8/lib"
    export PKG_CONFIG_PATH="/usr/local/opt/python@3.8/lib/pkgconfig"
    
### pyenv    
    brew install pyenv
    
### enable tab completion
    terraform -install-autocomplete
    
### terraform plan landscape macOS
    brew install terraform_landscape
    terraform plan ... | landscape

### Terraform commands
    terraform fmt
    terraform show
    terraform apply

### Consul
    https://releases.hashicorp.com/consul/1.9.4/consul_1.9.4_darwin_amd64.zip
    
### postman api
https://dl.pstmn.io/download/latest/osx

https://www.npmjs.com/package/newman 

`brew install newman`
        
https://www.npmjs.com/package/newman#command-line-options
        
https://www.npmjs.com/package/newman

### ssh cert for git 
    ssh-keygen -t ed25519 -C "email_address"
    eval "$(ssh-agent -s)"
    touch ~/.ssh/config
    open ~/.ssh/config
        Host *
        AddKeysToAgent yes
        UseKeychain yes
        IdentityFile ~/.ssh/id_ed25519    
        ssh-add -K ~/.ssh/id_ed25519
        
[git api](https://docs.github.com/en/free-pro-team@latest/rest/reference)

### get public ip
    my_ip=$(dig +short myip.opendns.com @resolver1.opendns.com);
    echo $my_ip

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

### Remove Speech Voices (876M	Voices)
```bash
cd /System/Library/Speech/
sudo rm -rf Voices/*
```
### Clear logs
`sudo rm -rf /private/var/log/*`

### User cache files
`cd ~/Library/Caches/`
