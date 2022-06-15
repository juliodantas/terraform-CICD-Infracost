# Manual Approval For Terraform Destroy
### Here for Release Destroy pipeline we will configure it for manual approval as it is going to be very sensitive & secured. 
### An unnecessarily or unwanted destroyed infrastructure can cause a huge loss of time, money, resources, backup & data. So we'll keep it highly secure to very limited number or reliable people.

## Step 1: Pre-deployment conditions
### Under Pre-deployment conditions we can give permissions to a very limited number of people who can give the manual approval for `terraform destroy`
![image](https://user-images.githubusercontent.com/99440004/173727812-db80fb77-a84a-40c6-80b2-a54c8f17e34d.png)


## Step 2: Pipeline Steps
### We will add these set of tasks under destroy pipeline
![image](https://user-images.githubusercontent.com/99440004/173728827-233ebd4b-50cd-4397-ba95-d3e16344c1bb.png)



## Step 3:
### This will install the dependency of Terraform with 1.1.8 version.
![image](https://user-images.githubusercontent.com/99440004/173728947-38e737b3-ecae-49ae-bdba-a83c2dc7bd98.png)
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-installer-task.TerraformInstaller@0

  displayName: 'Install Terraform 1.1.8'

  inputs:

    terraformVersion: 1.1.8
```


## Step 4:
### This step will initialize the terraform code in our system
### It will fetch the information from remote state file stored in Blob Storage. We also need an Active Azure Subscription Id for this step.
![image](https://user-images.githubusercontent.com/99440004/173728984-663d4e99-5199-4b20-a397-d3cc000ef7cf.png)
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-release-task.TerraformTaskV2@2

  displayName: 'Terraform : INIT'

  inputs:

    workingDirectory: '$(System.DefaultWorkingDirectory)/_IAC-CI/release'

    backendServiceArm: 'Opstree-PoCs (4c9****************3c)'

    backendAzureRmResourceGroupName: 'ADOagent_rg'

    backendAzureRmStorageAccountName: ter*********ort

    backendAzureRmContainerName: statefile

    backendAzureRmKey: terraform.tfstate
```


## Step 5:
### This step will auto destroy all the present configuration from our cloud
![image](https://user-images.githubusercontent.com/99440004/173729033-c372e9e5-e1b9-4bcf-8916-1aba861d756c.png)
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-release-task.TerraformTaskV2@2

  displayName: 'Terraform : DESTROY'

  inputs:

    command: destroy

    workingDirectory: '$(System.DefaultWorkingDirectory)/_IAC-CI/release'

    commandOptions: '--auto-approve'

    environmentServiceNameAzureRM: 'Opstree-PoCs (4c93ad0c-1cbc-4230-8418-f8df57418f3c)'

```