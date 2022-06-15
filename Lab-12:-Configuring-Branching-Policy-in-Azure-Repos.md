# Configuring Branching Policy in Azure Repos
### We need branching policies in order to save our main/master branch from unwanted commits.
### We can only Merge a branch into main/master branch using Pull Request if we want to make any changes into our main/master branch.
### Also to remember that our Terraform-CD or Release branch will only be triggered if Terraform-CI or Build branch was triggered from main branch.

## Step 1:
### For this we need to navigate to Project Settings then to Repositories and then select the repo where our code is stored.
![image](https://user-images.githubusercontent.com/99440004/173730073-384d1f11-a642-460e-b167-de01e01ffe46.png)

## Step 2: Branching Policy
### After selecting our repo we need to choose the policy section and then scroll down.
![image](https://user-images.githubusercontent.com/99440004/173730622-6b6f87a4-6479-41e7-b676-4169da26dfb7.png)


## Step 3: Configuring Branching Policy.
### Make the following changes & you're good to go.
![image](https://user-images.githubusercontent.com/99440004/173730797-fa227f7a-8dfc-4134-ab01-d239eb02c8fb.png)