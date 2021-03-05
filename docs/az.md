# az cli

[![Launch Cloud Shell](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)

## Run Azure CLI in Docker
`docker run -it mcr.microsoft.com/azure-cli`

`docker run -it -v ${HOME}/.ssh:/root/.ssh mcr.microsoft.com/azure-cli`

## Update Docker image
`docker pull mcr.microsoft.com/azure-cli`
    
## Authentication for Terraform
`az account set --subscription mySubscriptionName`

## az cli Terraform
`az account show`
```
vim main.tf    
    resource "azurerm_resource_group" "steve-dev-rg" {
             name = "steve-dev"
             location = "East US 2"
             }
```    

`terraform init`
`terraform plan`
`terraform apply`

`az group show -n steve-dev`
`az group create --location eastus --name steve-dev`

### Create an Ubuntu vm
`az vm create -n ubuntu01 -g steve-dev --image UbuntuLTS --generate-ssh-keys`

Using --generate-ssh-keys creates and setup public and private keys in the VM and 
$Home directory /home/<user>/.ssh/id_rsa and /home/<user>/.ssh/id_rsa.pub. 
.ssh folder is persisted in the Cloudshell's 5gb image used to persist $Home.

`az vm list-sizes --location eastus2`

## client_secret env vars for TF Azure Provider - store in keyvault
```
ARM_CLIENT_ID="00000000-0000-0000-0000-000000000000"
ARM_CLIENT_SECRET="00000000-0000-0000-0000-000000000000"
ARM_SUBSCRIPTION_ID="00000000-0000-0000-0000-000000000000"
ARM_TENANT_ID="00000000-0000-0000-0000-000000000000"
```
[Yeoman Github](https://github.com/azure/generator-az-terra-module)

[Create a Terraform base template in Azure using Yeoman](https://docs.microsoft.com/en-us/azure/developer/terraform/create-base-template-using-yeoman)

Install latest [Node LTS](https://nodejs.org). 
```
node --version
npm install -g yo
npm install -g generator-az-terra-module
yo --version
mkdir GeneratorDocSample
cd GeneratorDocSample
yo az-terra-module
./env_setup.sh
```
