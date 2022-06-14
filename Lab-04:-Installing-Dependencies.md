# Installing Dependencies
## Now our first tasks should be to install all the dependencies with their appropriate versions that are going to be directly or indirectly used while performing our further tasks.
## We're gonna need Docker, Azure CLI & Terraform in our Linux agent to perform our tasks.

## Step 1: Add following tasks into your Azure Pipeline.
![image](https://user-images.githubusercontent.com/99440004/173701820-6de95eae-6ab8-4c45-872b-5c6826e01ad5.png)

## Step 2: Installing Docker
![image](https://user-images.githubusercontent.com/99440004/173701968-1b74991f-3ff8-4998-97b2-e4a3e3ca530a.png)
### You can also use the YAML commands for the same task
```
steps:
- bash: |
   sudo apt update
   sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
   sudo apt update
   apt-cache policy docker-ce
   sudo apt install docker-ce -y
   sudo systemctl start docker

  displayName: 'Install Docker'
```

## Step 3: Install Azure CLI
![image](https://user-images.githubusercontent.com/99440004/173702193-2ec89a2a-2a47-4e7a-8c95-aca6ed76075a.png)
### You can also use the YAML commands for the same task 
```
steps:
- bash: |
   sudo apt-get update 
   sudo apt-get install ca-certificates curl apt-transport-https lsb-release gnupg -y
   curl -sL https://packages.microsoft.com/keys/microsoft.asc |
       gpg --dearmor |
       sudo tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null
   AZ_REPO=$(lsb_release -cs)
   echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |
       sudo tee /etc/apt/sources.list.d/azure-cli.list
   sudo apt-get update
   sudo apt-get install azure-cli -y
   sudo apt install unzip -y

  displayName: 'Install Azure CLI'

```

## Step 4: Install Terraform 1.1.8
![image](https://user-images.githubusercontent.com/99440004/173702508-68b62858-3827-48a1-8cb8-5c3f980eb307.png)
### You can also use the YAML commands for the same task 
```
steps:
- task: ms-devlabs.custom-terraform-tasks.custom-terraform-installer-task.TerraformInstaller@0

  displayName: 'Install Terraform 1.1.8'

  inputs:
    terraformVersion: 1.1.8
```

## Step 5: Pulling Required Docker Images
![image](https://user-images.githubusercontent.com/99440004/173702675-f5c85e68-c9ce-4b21-bda9-7e72a990ea7b.png)
### You can also use the YAML commands for the same task 
```
steps:
- bash: |
   # TFSEC
   sudo docker pull tfsec/tfsec:v1.13.2-arm64v8
   
   # TFLINT
   sudo docker pull ghcr.io/terraform-linters/tflint:v0.35.0
   
   # InfraCost
   sudo docker pull infracost/infracost:0.9
   

  displayName: 'Pulling Required Docker Images'


```