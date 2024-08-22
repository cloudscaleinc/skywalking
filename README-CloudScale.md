# SkyWalking Azure Deployment

This will help you setting up the SkyWalking project on an Azure VM using Docker for installation.

## Prerequisites

- **Java**: Java Development Kit (JDK) 11 or 20
- **Maven**: Install Maven from Maven's official site
- **Git**: Make sure Git is installed on local machine.
- **Docker**: Ensure Docker and Docker Compose are installed.
- **Azure CLI**: Install the Azure CLI to manage Azure resources.

## 1. Clone the Repository

Clone the SkyWalking repository to the local machine:
```sh
git clone https://github.com/repo/skywalking.git
cd skywalking
```

## Checkout the Specific Branch
```sh
git checkout skywalking-azure
```

## Initialize Submodules
```sh
git submodule update --init --recursive
```

## Prepare Docker Environment
```sh
make docker
```

## Build and Run the Containers
```sh
docker-compose --profile elasticsearch up -d
```

## Log in to Azure
```sh
az login
```

## Create a Resource Group
```sh
az group create --name skywalking-rg --location centralindia
```

## Create an Azure VM
```sh
az vm create --resource-group skywalking-rg --name skywalking-vm --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
```

## Open Port 8080 on the Azure VM
```sh
az vm open-port --resource-group skywalking-rg --name skywalking-vm --port 8080
```

## SSH into the Azure VM
```sh
ssh azureuser@vm-ip
```

## Pull the Docker Image inside the VM
```sh
docker pull docker-image
```

## Run the Docker Container
```sh
docker-compose --profile elasticsearch up -d
```
## Configure GitHub Secrets

Add the secrets
```sh
SSH_PRIVATE_KEY : key used for accessing the VM
VM_IP: The public IP address of the VM.
VM_USER: The SSH username for the VM.
```
## Create the GitHub Container
Generate a github Personal access token :
store its value in PAT
```sh
echo $PAT | docker login ghcr.io -u username --password-stdin
```
