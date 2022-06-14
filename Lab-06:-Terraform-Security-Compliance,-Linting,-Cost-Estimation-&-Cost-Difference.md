# Terraform Security Compliance, Linting, Cost Estimation & Cost Difference 
## In order to further examine our Terraform code for better re-usability, security & cost estimation we'll add following checks into our Azure Pipeline.

## Step 1: Add Security Compliance, Linting, Cost Estimation & Cost Difference in your pipeline
![image](https://user-images.githubusercontent.com/99440004/173705335-bc06a2c7-a870-4923-9b66-b0dbd09763c3.png)


## Step 2: Terraform | TFSEC
![image](https://user-images.githubusercontent.com/99440004/173705458-bb4c66bb-f71b-4c16-8f9b-b4bc9a43287f.png)
### Here we are using `Tfsec` as a terraform compliance tool for our code. 
### Using this tool we can configure our custom checks for our code and can monitor their compliance.
### A Custom checks file will be stored in our code in following way:
![image](https://user-images.githubusercontent.com/99440004/173706000-5eacc4df-21e4-4f85-834a-19194b2169e2.png)
```
---
checks:
  - code: CUS001
    description: Custom check to ensure the Name tag is applied to Resources Group Module 
    impact:  By not having Name Tag we can't keep track of our Resources
    requiredTypes:
      - module
    requiredLabels:
      - resource_group
    severity: MEDIUM
    matchSpec:
     name: tag_map
     action: contains
     value: Name
    errorMessage: The required Name tag was missing

  - code: CUS002
    description: Custom check to ensure the Name tag is applied to Resources Group Module
    impact:  By not having Environment Tag we can't keep track of our Resources
    requiredTypes:
      - module
    requiredLabels:
      - resource_group
    severity: CRITICAL
    matchSpec:
      name: tag_map
      action: contains
      value: Environment
    errorMessage: The required Environment tag was missing

  - code: CUS003
    description: Custom check to ensure Resource Group is going to be created in Australia East region
    impact:  By not having our resource in Australia East we might get some latency
    requiredTypes:
      - module
    requiredLabels:
      - resource_group
    severity: MEDIUM
    matchSpec:
     name: resource_group_location
     action: equals
     value: "Australia East"
    errorMessage: The required "Australia East" location was missing

  - code: CUS004
    description: Custom check to ensure that suffix applied to All the Resource groups
    impact:  By not having suffix we can't keep track of our Resources
    requiredTypes:
      - module
    requiredLabels:
      - resource_group
    severity: MEDIUM
    matchSpec:
      name: resource_group_name
      action: endsWith
      value: Opstree-POC
    errorMessage: The required suffix "Opstree-POC" was missing

  - code: CUS005
    description: Custom check to ensure that suffix applied to All the Virtual Networks
    impact:  By not having suffix we can't keep track of our Virtual Networks
    requiredTypes:
      - module
    requiredLabels:
      - vnet
    severity: MEDIUM
    matchSpec:
      name: vnet_name
      action: endsWith
      value: Opstree-POC
    errorMessage:  The required suffix "Opstree-POC" was missing
```
### You can also use the YAML commands for the same task
```
steps:
- bash: 'sudo docker run --rm -v "$(pwd):/src" aquasec/tfsec /src --tfvars-file /src/terraform.tfvars'
  displayName: 'Terraform : TFSEC'
  condition: succeededOrFailed()
``` 

## Step 3: Terraform | Linting
![image](https://user-images.githubusercontent.com/99440004/173706552-69272e3d-5a15-49bf-bfdf-83b117d180ff.png)
### Here we are using a docker image of `tflint` as a tool to lint out Terraform Code
### You can also use the YAML commands for the same task
```
steps:
- bash: |
   sudo docker run --rm -v $(pwd):/data -t ghcr.io/terraform-linters/tflint
   
  displayName: 'Terraform : Linting'
  condition: succeededOrFailed()
``` 

## Step 4: Terraform | Cost Estimation
![image](https://user-images.githubusercontent.com/99440004/173706692-b3de8aaa-9125-4e7d-b3cd-ecaba3c04501.png)
### Here we are using a docker image of `infracost` as a tool to calculate our estimated cost for our terraform code
### For this you need to generate an API key.
### To generate the API key you can use `infracost register` command to register yourself using your Username & E-mail.
### You can also use the YAML commands for the same task
```
steps:
- bash: |
   terraform show -json plan.out > plan.json
   
   sudo docker run --rm   -e INFRACOST_API_KEY=$(INFRACOST_API_KEY)   -v "$(pwd):/src" infracost/infracost breakdown --path  /src/plan.json --show-skipped 
   
  displayName: 'Terraform : Cost Estimation'
  condition: succeededOrFailed()

``` 

## Step 5: Terraform | Cost Difference
![image](https://user-images.githubusercontent.com/99440004/173707386-2f2ab103-8496-492e-aed3-5d5d07a24196.png)
### Using same previous tool `infracost` to calculate the difference between our present state & desired state of our configuration
### You can also use the YAML commands for the same task
```
steps:
- bash: |
   terraform show -json plan.out > plan.json
   
   sudo docker run --rm   -e INFRACOST_API_KEY=$(INFRACOST_API_KEY)   -v "$(pwd):/src" infracost/infracost diff --path  /src/plan.json --show-skipped
   
  displayName: 'Terraform : Cost Difference'
  condition: succeededOrFailed()

``` 
