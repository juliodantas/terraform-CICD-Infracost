# Generating & Uploading Logs
## We need to generate our important reports for future use and upload it to a storage drive to store it securely. 
## Step 1: Add following steps in your Azure Pipeline.
![image](https://user-images.githubusercontent.com/99440004/173716799-b0ae8ffc-0157-465f-9fba-7a07e429ce81.png)

### 

## Step 2: Generating Logs 
![image](https://user-images.githubusercontent.com/99440004/173719293-8966c052-44a8-4016-8539-4e8efd149ed8.png)
### Here We are generating logs for Terraform Compliance, Cost Estimation & Cost difference. 
### You can also use the YAML commands for the same task
```
steps:
- bash: |
   # Creating Logs Of Tfsec
   sudo docker run --rm -v "$(pwd):/src" aquasec/tfsec /src --tfvars-file /src/terraform.tfvars > $(Build.DefinitionName)-tfsec-$(Build.BuildNumber)
   
   #Creating Logs Of Cost Estimation
   sudo docker run --rm   -e INFRACOST_API_KEY=$(INFRACOST_API_KEY)   -v "$(pwd):/src" infracost/infracost breakdown --path  /src/plan.json --show-skipped --format html > $(Build.DefinitionName)-cost-$(Build.BuildNumber).html
   
   #Creating Logs Of Cost Diffrence
   sudo docker run --rm   -e INFRACOST_API_KEY=$(INFRACOST_API_KEY)   -v "$(pwd):/src" infracost/infracost diff --path  /src/plan.json --show-skipped > $(Build.DefinitionName)-cost-diff-$(Build.BuildNumber)
   
  displayName: 'Generating Logs'
  condition: succeededOrFailed()

``` 

## Step 3: Upload tfsec file
![image](https://user-images.githubusercontent.com/99440004/173720015-88a5f067-ca8a-4571-942e-f55416662c95.png)
### Here we are uploading our compliance report to our storage account in our Azure Container Blob Storage
### To upload this report you'll need an Active Azure Subscription & Azure Container Blob Storage
### You can also use the YAML commands for the same task
```
steps:

- task: AzureCLI@2

  displayName: 'Upload tfsec file '

  inputs:

    azureSubscription: 'Opstree-PoCs (4c*************************f3c)'

    scriptType: bash

    scriptLocation: inlineScript

    inlineScript: |
     az storage blob upload --file $(Build.DefinitionName)-tfsec-$(Build.BuildNumber) --name $(Build.DefinitionName)-tfsec-$(Build.BuildNumber) --account-name terr*********port --container-name report
     

    workingDirectory: report

  condition: succeededOrFailed()


``` 

## Step 4: Upload cost file
![image](https://user-images.githubusercontent.com/99440004/173720146-2ff88325-daaa-4e48-8fd1-f7848feb4d46.png)
### Here we are uploading our cost estimation report in HTML format to our storage account in our Azure Container Blob Storage
### To upload this report you'll need an Active Azure Subscription & Azure Container Blob Storage 
### You can also use the YAML commands for the same task
```
steps:

- task: AzureCLI@2

  displayName: 'Upload cost file '

  inputs:

    azureSubscription: 'Opstree-PoCs (4c9**********************3c)'

    scriptType: bash

    scriptLocation: inlineScript

    inlineScript: 'az storage blob upload --file $(Build.DefinitionName)-cost-$(Build.BuildNumber).html --name $(Build.DefinitionName)-cost-$(Build.BuildNumber).html --account-name ter************port --container-name report'

    workingDirectory: report

  condition: succeededOrFailed()


``` 

## Step 5: Upload cost diff file
![image](https://user-images.githubusercontent.com/99440004/173720311-13bcc1af-1ecb-41e1-a521-6a7873ba45f2.png)
### Here we are uploading our cost comparison between present state & desired state report to our storage account in our Azure Container Blob Storage
### To upload this report you'll need an Active Azure Subscription & Azure Container Blob Storage 
### You can also use the YAML commands for the same task
```
steps:

- task: AzureCLI@2

  displayName: 'Upload cost diff file'

  inputs:

    azureSubscription: 'Opstree-PoCs (4c9*******************f3c)'

    scriptType: bash

    scriptLocation: inlineScript

    inlineScript: 'az storage blob upload --file $(Build.DefinitionName)-cost-diff-$(Build.BuildNumber) --name $(Build.DefinitionName)-cost-diff-$(Build.BuildNumber) --account-name ter***************port --container-name report'

    workingDirectory: report

  condition: succeededOrFailed()


``` 
