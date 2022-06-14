# Configuring Your First Self Hosted Agent
## Step 1:
### Click to Project Settings at bottom left corner
![image](https://user-images.githubusercontent.com/99440004/173686097-3d83fc5e-e1a4-4299-b0e1-dc597c231dc8.png)

## Step 2:
## Go to Agent Pools under Pipelines header
![image](https://user-images.githubusercontent.com/99440004/173686292-ab91ad04-0829-4b7a-a682-420f927ff38f.png)

## Step 3:
### Click Add Pool and choose Self Hosted Agent
![image](https://user-images.githubusercontent.com/99440004/173686613-04fe32ca-2b01-4f36-aab7-652cc25543a4.png)

## Step 4:
### Now move inside your newly created pool & click at New Agent
### Choose Your os-flavor and move forward accordingly.
![image](https://user-images.githubusercontent.com/99440004/173686958-13335815-b73b-4c21-bc9a-a7872f5fb55f.png)
### You can also use following commands to configure your linux agent for ADO
```mkdir myagent
cd myagent
wget https://vstsagentpackage.azureedge.net/agent/2.204.0/vsts-agent-linux-x64-2.204.0.tar.gz
tar zxvf vsts-agent-linux-x64-2.204.0.tar.gz
```

## Step 4: Configure your first linux Agent
### Run `./config.sh` in your terminal
![image](https://user-images.githubusercontent.com/99440004/173689503-61f1f626-886a-4995-ab52-a54c0bc61050.png)
### Now you need to generate a Personal Access Token (PAT)
For that you need to go at Personal Access Tokens section under User Settings located at top right corner of your screen.
Name it accordingly and grant permissions as per your requirements
![image](https://user-images.githubusercontent.com/99440004/173690130-c6123569-7171-4644-b5d8-e4c6008c9577.png)
### Warning - Make sure you copy the token. Azure don't store it and you will not be able to see it again. 
### PAT can only be regenerated 

![image](https://user-images.githubusercontent.com/99440004/173690535-4fc9e1c0-640b-4b9e-b22a-c31057d18e67.png)

## Step 5:
### Now paste that PAT in your terminal
### Enter agent pool (press enter for default)
### Enter agent name (press enter for linux)
### Enter work folder (press enter for _work)
And you're done with your ADO Self Hosted Agent Configuration
![image](https://user-images.githubusercontent.com/99440004/173691384-096dbd11-6653-45b0-bdca-6563016f2721.png)


## Step 6:
### Now Run `./run.sh` to bring your agent from Offline to Online
![image](https://user-images.githubusercontent.com/99440004/173691913-2a39ebd9-e5ff-4795-b2a0-9aa81f95b7a0.png)
![image](https://user-images.githubusercontent.com/99440004/173691970-9a4f7de8-a206-4477-8cf9-b3a7eab1c7f2.png)
![image](https://user-images.githubusercontent.com/99440004/173692005-94c091b9-70ce-4b7a-bf7d-2d998f770d69.png)