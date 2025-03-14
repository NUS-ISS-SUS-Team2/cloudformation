# cloudformation
Repo for cloudformation yaml files

This template pulls Springboot backend app from Github and deploys it to an EC2 instance.

Using CloudFormation, there is an ability to instantly spin up all necessary AWS resources from the template without needing to configure everything from scratch. The even more amazing thing is that you can delete the stack and AWS will tear down all created resources (though have to manually delete some like ECR and empty S3 first) to not incur costs when done with testing.

CloudFormationSpring.yaml

Prequisites needed:
  Parameters:
  - Description: (Name of the cloudformation)
  - github repo and branch
  - ECR repository name (I named it same as Github's)
  - Below 3 params AWS will provide a drop down to current AWS resources:
    - EC2 Keypair generated and in AWS console (via EC2)
    - VPC created (ID)
    - Subnet ID

Current template will spin up:
 - One EC2 instance (script to install docker and codedeploy for this EC2 within this template as well)
 - One ECR Repo (as i understand to act as what dockerhub does)
 - Codepipeline pipeline (Current pipeline trigger is still manual, through AWS console triggering the full pipeline on release change)
 - Codebuild to get from Github source (need generate a oauthtoken to allow the pull), build the dockerfile into a dockerized container     and push to ECR. (buildspec.yml within project root)
 - Codedeploy deployment (appspec.yml within the project) to run scripts in /scripts to stop/start the docker container in the EC2


Update 14 Mar 25

RdsCloudFormation.yml

Cloudformation for RDS separately deployed in RdsCloudFormation.yml (run as a stack first)
  - This creates a simple lowest cost empty RDS instance (mysql)
  - To copy this RDS endpoint into the springboot applications.properties then commit to github

Notes:
EC2 springboot docker image now runs and EC2publicip/order/getAllOrders returns DB data response. 

TO DO:
  - Test get data from APIgateway + ALB
  - Web hooks for github detecting pushes and trigger pipeline automatically
  - Integrating Sonarqube (static analysis) & OWASP zap (dynamic analysis) into pipeline (can integrate from cloudformation level)
  - Cloud watch monitoring
  - Check additional security measures AWS can spin up from cloudformation to integrate