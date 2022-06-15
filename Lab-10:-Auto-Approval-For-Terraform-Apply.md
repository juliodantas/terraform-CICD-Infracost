# Auto Approval For Terraform Apply

### 

## Step 1: Pre-deployment conditions
### Under Pre-deployment conditions we will go with After Release.
### Apply Pipeline will be triggered as soon as Release Artifact will be generated. 
![image](https://user-images.githubusercontent.com/99440004/173725762-218c3c20-2976-4357-9f6c-25a2ae2bf783.png)

## Step 2: Deploy Pipeline
### It will contain following steps:
![image](https://user-images.githubusercontent.com/99440004/173726120-daa9c93c-dca9-42a4-b984-e73057dc04f0.png)

## Step 3: Install Terraform
### This will install the dependency of Terraform with 1.1.8 version.
![image](https://user-images.githubusercontent.com/99440004/173726300-4bf857b6-b1cb-4eb5-bfbd-c42bbb15dc77.png)
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-installer-task.TerraformInstaller@0

  displayName: 'Install Terraform 1.1.8'

  inputs:

    terraformVersion: 1.1.8
```

## Step 4: Terraform : INIT
### This step will initialize the terraform code in our system
### It will fetch the information from remote state file stored in Blob Storage. We also need an Active Azure Subscription Id for this step.
![image](https://user-images.githubusercontent.com/99440004/173726241-bb12659a-c4c1-4c66-b50a-f00abdcd50f7.png)
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

## Step 5: Terraform : APPLY
### This step will auto apply all the desired configuration into our cloud
![image](https://user-images.githubusercontent.com/99440004/173726355-804b80d5-b9a7-4c14-a156-9b25384b27b6.png)
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-release-task.TerraformTaskV2@2

  displayName: 'Terraform : APPLY'

  inputs:

    command: apply

    workingDirectory: '$(System.DefaultWorkingDirectory)/_IAC-CI/release'

    commandOptions: '-auto-approve'

    environmentServiceNameAzureRM: 'Opstree-PoCs (4c93****************3c)'

```