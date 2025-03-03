# cloudformation
Repo for cloudformation yaml files

This template pulls Springboot backend app from Github and deploys it to an EC2 instance.

Prequisites needed:
  Parameters:
  - Description: (Name of the cloudformation)
  - github repo and branch
  - ECR repository name (I named it same as Github's)
    Below 3 params AWS will provide a drop down to current AWS resources:
  - EC2 Keypair generated and in AWS console (via EC2)
  - VPC created (ID)
  - Subnet ID

Current template will spin up:
 - One EC2 instance (script to install docker and codedeploy for this EC2 within this template as well)
 - One ECR Repo (as i understand to act as what dockerhub does)
 - Codepipeline pipeline 
 - Codebuild to get from Github source (need generate a oauthtoken to allow the pull), build the dockerfile into a dockerized container     and push to ECR. (buildspec.yml within project root)
 - Codedeploy deployment (appspec.yml within the project) to run scripts in /scripts to stop/start the docker container in the EC2
