### Install Homebrew 
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
    
    brew --version
    
### Install developer tools
    xcode-select --install

### Openssl
    brew install openssl
    
    'export LDFLAGS="-L/usr/local/opt/openssl@1.1/lib"'
    'export CPPFLAGS="-I/usr/local/opt/openssl@1.1/include"'
    'echo 'export PATH="/usr/local/sbin:$PATH"' >> ~/.zshrc'
  
    brew doctor
    brew cleanup

### Magic packet
    brew install wakeonlan
    
  ## macOS setup notes

    curl -O https://releases.hashicorp.com/terraform/0.14.7/terraform_0.14.7_darwin_amd64.zip
    unzip terraform_0.14.7_darwin_amd64.zip
    mv terraform /usr/local/bin/
    rm terraform_0.14.7_darwin_amd64.zip
    
    echo $PATH

    brew update && brew install azure-cli
    brew update && brew upgrade azure-cli
    
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

### postman api
   https://dl.pstmn.io/download/latest/osx

   https://www.npmjs.com/package/newman 
        
        brew install newman
        
   https://www.npmjs.com/package/newman#command-line-options
        
   https://www.npmjs.com/package/newman

### 1Password cli
    
[1Password cli](https://support.1password.com/command-line/)

[1Password cli reference](https://support.1password.com/command-line-reference/)

    brew install --cask 1password-cli
    
    op signin my.ent.1password.com <email_address> <secret_key> [--raw]
    export OP_SESSION_my"KEY"
    eval $(op signin my)
    op list items
    
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


