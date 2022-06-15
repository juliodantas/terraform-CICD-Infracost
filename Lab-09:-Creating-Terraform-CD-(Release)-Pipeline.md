# Creating Terraform CD (Release) Pipeline
### Now finally it's time to deploy our terraform code.
### One thing to note here is that, this Release (Terraform-CD) pipeline will only be triggered once our CI pipeline will be successfully finished passing all the checks. 
### As our CI pipeline will be successfully completed, it'll generate a Release Artifact which will further auto trigger our CD or Release or Deploy pipeline. 

## Step 1: Navigate to Release Pipeline under Pipeline column.
### Choose the latest artifact from your Terraform-CI pipeline.
![image](https://user-images.githubusercontent.com/99440004/173723719-40521df6-a735-4eed-a016-8e991fbcef02.png)

## Step 2: Configure Continuous Deployment Trigger
![image](https://user-images.githubusercontent.com/99440004/173724642-f4b269df-fb0f-4cd7-a0c8-a06c5e92434b.png)
### Enable for CD Trigger & configure for Main branch as build branch

