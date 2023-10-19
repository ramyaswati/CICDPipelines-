#This contains detailed documentation of the steps and screenshot of the steps followed to achive the goal.

## Solution As Part of solution we are creating Elastic container service ECS and deploy using AWS CodePipelines. 

### Initial code commit and creating of Elastic container service ECS 

## Step 1 : Created a Git Repository in git hub and commited the locally build node js project code to my git repo.
## Step 2: Created ECR Repo in AWS > build the docker image locally , login to ECR , tag the image and push to AWS ECR .
    docker build -t nodeapp:latest .
    #Login to the ECR repository 
    aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/j8l2c6s6/nodeapp
    #Tag the Docker image with the ECR repository image:
    docker tag node-hello-world:latest public.ecr.aws/m9h7v0h3/node-hello-world:latest
    #Finally, push the image to the ECR repository:
    docker push public.ecr.aws/m9h7v0h3/node-hello-world:latest
## Step 3:Created a load balancer and target group with security group (Allow the traffic on port 80 or 443 from internet 
    Protocol: HTTP on PORT: 3000 
    Listener on HTTP port 80
## Step 4: Created ECS Cluster > 
    Name: myCluster > 
    Infrastructure: AWS fargate(serverless)
## Step 5: Created task-definition ,to use for creating services inside the cluster >
    Task size: 0.5 CPU and 1 GB Memory . 
    Container port 3000 as our application is configured to run on this port.
## Step 6: Created ECS service >
    Application Type: Service >    
    Service name: my-ecs-service> 
    Service Type: Replica > 
    Created security group to expose port 3000 of this service and which is accessible only by the load balancer security group> 
    Select the load balancer, listener & target group .
    And Finally create the ECS .
## Step 7: Check application , Open the load balancer and check the application status

### Setting up AWS CodePipeline

## Step 1: Created New AWS CodePipelines named ecs-pipeline 
## Step 2: Configured the Source Stage with my Git repository mapping
## Step 3: configure the codebuild project >
    Project Name: my-ecs-codebuild >    
    Environment Image: Managed Image >
    Environment type: Linux EC2  >
    Build specifications: Use a buildspec file > 
    Buildspec name: build-spec.yml (from code repo)
## Step 4: Add the build Stage 
## Step 5: Add the Deploy Stage > 
    cluster name >  created earlier  
    service name > create earlier
## Step 6: Review and Create Pipeline 

# Netwok security configuration screenshot added to Images .

## Closure : This will automatically trigger the CodePipeline, which will build the Docker image, push it to Amazon ECR, and update the ECS service with the new image.
