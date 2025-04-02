# cloudformation
Repo for cloudformation yaml files

This template pulls Springboot backend app from Github and deploys it to an EC2 instance.

Using CloudFormation, there is an ability to instantly spin up all necessary AWS resources from the template without needing to configure everything from scratch. This Infra as code (IAC) should be one of the secure foundations as it serves as a backup of cloud configurations and also orchestrates the AWS resources together.

CloudFormationSpring.yaml

Prequisites needed:
- Parameters:
  - Description: (Name of the cloudformation)
  - github repo and branch
  - ECR repository name (I named it same as Github's)
  - Below 3 params AWS will provide a drop down to current AWS resources:
    - EC2 Keypair generated and in AWS console (via EC2)
    - VPC created (ID)
    - Subnet ID
  - (The other parameters are defaulted to AWS resources to save time on typing each one)

Current template will spin up:
 - One EC2 instance (script to install docker, codedeploy & mysql for this EC2 within this template as well)
 - One ECR Repo (as i understand to act as what dockerhub does)
 - Codepipeline pipeline (Current pipeline trigger is still manual, through AWS console triggering the full pipeline on release change)
 - Codebuild to get from Github source (need generate a oauthtoken to allow the pull), build the dockerfile into a dockerized container and push to ECR. (buildspec.yml within project root)
 - Codedeploy deployment (appspec.yml within the project) to run scripts in /scripts to stop/start the docker container in the EC2

Update 14 Mar 25

RdsCloudFormation.yml

Cloudformation for RDS separately deployed in RdsCloudFormation.yml (run as a stack first)
  - This creates a simple lowest cost empty RDS instance (mysql)
  - To copy this RDS endpoint into the springboot applications.properties then commit to github

As of 3 Apr:
- Apigateway data getAllOrders success. Route53 subdomain cert covers https for apigateway and ALB. As such, can use subdomain/route url to access resource
- Sonarqube analysis for springboot built into codepipeline (Analysis viewed on sonarcloud)
- OWASP zap scan for API url build into codepipeline to take place after api is deployed. (Scan report logged in S3 bucket)
- Cloudwatch monitoring set up, currently logs of requests log for API
- WAF setup, currently allows all. Log group for blocked requests also setup.

Cloudformations TODO:
  - Security token from frontend to access APIs (Looks simple to implement but need access to the other AWS cognito.)
  - To test WAF to see logs for blocked requests.