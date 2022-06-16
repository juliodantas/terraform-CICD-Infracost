# Generating Artifacts
### We need so save the output or by-product of our Terraform-CI pipeline. Here in this case we will save two artifacts.
### We will create one report artifact for our logs generated in our previous lab.
### One more artifact will be created named release which will contain all the files with `.tf` or `.tfvars` extention required by terraform.

## Step 1: We will add these steps in our Azure Pipeline.
![image](https://user-images.githubusercontent.com/99440004/173721074-dfcc1dd0-5d83-447e-ad21-1441ea3baabf.png)

## Step 2: Copy Files to: release
![image](https://user-images.githubusercontent.com/99440004/173721940-3b10bb80-8050-42ec-8505-c81e2e768da2.png)
### This will copy all the `.tf` `.tfvars` file extention into release directory
### You can also use the YAML commands for the same task
```
steps:
- task: CopyFiles@2
  displayName: 'Copy Files to: release'
  inputs:
    SourceFolder: '$(Pipeline.Workspace)/s'
    Contents: '*.tf*'
    TargetFolder: '$(Pipeline.Workspace)/s/release'
    OverWrite: true

``` 

## Step 3: Copy Files to: report
![image](https://user-images.githubusercontent.com/99440004/173722066-3af01ea4-b9ee-44a2-a13b-6cd01cb7345c.png)
### This will copy all the generated logs into reports directory
### You can also use the YAML commands for the same task
```
steps:
- task: CopyFiles@2
  displayName: 'Copy Files to: report'
  inputs:
    SourceFolder: '$(Pipeline.Workspace)/s'
    Contents: '*$(Build.DefinitionName)*'
    TargetFolder: '$(Pipeline.Workspace)/s/report'

``` 

## Step 4: Publish Artifact: Release
![image](https://user-images.githubusercontent.com/99440004/173722144-bed9a47d-d86e-4978-a8bd-dff2e84a8ae2.png)
### Publishing Release Artifact
![image](https://user-images.githubusercontent.com/99440004/173723347-9bf1290a-28a7-4717-9567-22083d50fbd7.png)
### This Release Artifact will trigger the Release (Terraform-CD) Pipeline.
### If in case pipeline fails, Release Artifact will not be generated hence Release Pipeline will not be triggered.
### You can also use the YAML commands for the same task
```
steps:
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: Release'
  inputs:
    PathtoPublish: '$(Pipeline.Workspace)/s/release'
    ArtifactName: release
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
``` 

## Step 5: Publish Artifact: report
![image](https://user-images.githubusercontent.com/99440004/173722211-23f331f8-2f44-4bda-aaa1-28409fb92574.png)
### Publishing Report Artifact
![image](https://user-images.githubusercontent.com/99440004/173723443-5dacb622-022c-47b1-b316-03f56185e6b9.png)
### You can also use the YAML commands for the same task
```
steps:

- task: PublishBuildArtifacts@1

  displayName: 'Publish Artifact: report'

  inputs:

    PathtoPublish: report

    ArtifactName: report

  condition: succeededOrFailed()


``` 