# az cli

[![Launch Cloud Shell](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)

## Run Azure CLI in Docker

    docker run -it mcr.microsoft.com/azure-cli

    docker run -it -v ${HOME}/.ssh:/root/.ssh mcr.microsoft.com/azure-cli  

## Update Docker image
    
    docker pull mcr.microsoft.com/azure-cli
    
## Authentication for Terraform

    az account set --subscription mySubscriptionName

## az cli Terraform

    az account show
    
    vim main.tf
    
             resource "azurerm_resource_group" "steve-tf-rg" {
             name = "steve-dev"
             location = "East US 2"
             }
    
    terraform init
    
    terraform plan
    
    terraform apply


az group show -n Steve-Dev-Sandbox

az group create --location eastus --name steve-dev   

### az vm create -n ubntu01 -g Steve-Dev-Sandbox --image UbuntuLTS --generate-ssh-keys
    Using --generate-ssh-keys creates and set ups public and private keys in the VM and 
    $Home directory /home/<user>/.ssh/id_rsa and /home/<user>/.ssh/id_rsa.pub. 
    .ssh folder is persisted in the Cloudshell's 5-GB image used to persist $Home.

az vm list-sizes --location eastus2

## client_secret vars for TF Azure Provider - store in vault
    $ export ARM_CLIENT_ID="00000000-0000-0000-0000-000000000000"
    $ export ARM_CLIENT_SECRET="00000000-0000-0000-0000-000000000000"
    $ export ARM_SUBSCRIPTION_ID="00000000-0000-0000-0000-000000000000"
    $ export ARM_TENANT_ID="00000000-0000-0000-0000-000000000000"
