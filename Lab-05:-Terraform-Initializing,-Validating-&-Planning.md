# Terraform Initializing, Validating & Planning
## Now in order to validate & fetch cost estimation we need to initialize & plan for our Terraform Code.
 
## Step 1: Add following tasks into your Azure Pipeline
![image](https://user-images.githubusercontent.com/99440004/173702983-f18c1b0e-0425-4bcd-9733-c0276b55359c.png)
 
## Step 2: Terraform | Init
![image](https://user-images.githubusercontent.com/99440004/173704059-2847a386-bf85-4860-bcdf-b5da434a64eb.png)
### Here under this section you need to authorize your Azure Subscription to configure for your Backend Service Arm or your Backend State File to maintain a stable configuration of your infrastructure. So keep a Blob Storage Container in your Azure Subscription handy.
### You can also use the YAML commands for the same task
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-release-task.TerraformTaskV2@2

  displayName: 'Terraform : INIT'

  inputs:

    backendServiceArm: 'Opstree-PoCs (4c***************************f3c)'

    backendAzureRmResourceGroupName: 'ADOagent_rg'

    backendAzureRmStorageAccountName: terr**********report

    backendAzureRmContainerName: statefile

    backendAzureRmKey: terraform.tfstate


```

## Step 3: Terraform | Validate
![image](https://user-images.githubusercontent.com/99440004/173704550-3353f78d-7486-4b20-a751-7294b3b3cbcf.png)
### Use the `terraform validate` command to validate your terraform code
### You can also use the YAML commands for the same task
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-release-task.TerraformTaskV2@2

  displayName: 'Terraform : VALIDATE'

  inputs:

    command: validate


```

## Step 4: Terraform | Plan
![image](https://user-images.githubusercontent.com/99440004/173704850-2afa55b2-313e-4a30-9f6f-ec399bfd34ef.png)
### Use the `terraform plan -lock=false -out=plan.out` command to plan your terraform code with a output file.
### Here again you need to provide your Azure Subscription id
### You can also use the YAML commands for the same task
```
steps:

- task: ms-devlabs.custom-terraform-tasks.custom-terraform-release-task.TerraformTaskV2@2

  displayName: 'Terraform : PLAN ( For Cost Optimization )'

  inputs:

    command: plan

    commandOptions: '-lock=false -out=plan.out'

    environmentServiceNameAzureRM: 'Opstree-PoCs (4c**************************3c)'


```