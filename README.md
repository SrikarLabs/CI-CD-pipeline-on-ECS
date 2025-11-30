This project focuses on setting up a CI/CD pipeline for deploying a Dockerized version of the 2048 game to AWS. The pipeline automates the process of building, testing, and deploying the game to a scalable infrastructure using Amazon ECS, Amazon ECR, and AWS CodePipeline.

<img width="2036" height="810" alt="image" src="https://github.com/user-attachments/assets/e33fa299-9a74-45da-ac67-026f2db9e62e" />

	Key tasks include:
		• Building and pushing a Docker image of the 2048 game to Amazon ECR.
		• Setting up a CodePipeline to automate the build and deploy process.
		• Deploying the Dockerized game to Amazon ECS using a Fargate launch type.
	In this lab, you'll learn how to integrate various AWS services to create a streamlined CI/CD workflow, reducing manual intervention and enabling faster, reliable application deployment.
	
	Steps to be performed 👩‍💻
	We'll go through the following steps in the next few lessons.
	1. Set Up ECS Cluster and ECR Repository
	2. Prepare the 2048 Game Code
	3. Set Up CodeBuild for Continuous Integration
	4. Set Up CodePipeline for Continuous Deployment
	
	Services Used 🛠
		• AWS CodePipeline: Orchestrates the CI/CD pipeline, automating the build, test, and deployment stages. [Automation]
		• Amazon ECS Deploys and manages containerized applications using Fargate for serverless container management. [Deployment]
		• Amazon ECR: Stores and manages Docker images used in the ECS tasks. [Container Registry] 
		• AWS CodeBuild: Handles the build phase of the pipeline, including Docker image creation.[Build]
		• IAM Roles & Policies: Ensure secure access between the services involved. [Permissions]
<img width="1318" height="1332" alt="image" src="https://github.com/user-attachments/assets/bc8b7727-f350-479e-8017-f0b84b44f719" />


	
Steps:
	1. Install Docker and AWS CLI (configure with IAM User Access key)
	2. Create Docker file
	
		Docker file
		
	3. Build docker image from this Docker file -> docker build -t 2048-game .
	4. Push the Docker image to ECR
		a. Tag
			docker tag 2048-game:latest <ECR_URI>:latest (create ECR URI from console or command) -> Tagging gives name and version to Image
		b. Authenticate with ECR
			aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <ECR_URI>
		c. Push the image
			docker push <ECR_URI>:latest
		d. Verify the image in ECR (console)
	5. Access website with ECS url
		a. Create ECS Service-linked Role
		b. Create ECS cluster
		c. Register Task Definition -> under Container Definition - add Image URI
		d. Create Service -> set up SG
		e. Get Public IP from -> ECS Cluster -> Task -> Networking -> Browser this URL http://<Public-IP>:80


Set up CI/CD pipeline
	
	1. Build buildspec.yml file in the same repo as code
		Yml file contains steps which are done manually above to build image, push to ECR, deploy image to ECS
		
			
			
			- install phase: Specifies the Docker version to use.
			
			- build phase: Builds the Docker image, logs into ECR, tags the image, and prepares it for pushing.
			
			- post_build phase: Pushes the Docker image to ECR and generates the imagedefinitions.json file, which ECS uses for deployments.
			
			- artifacts: The imagedefinitions.json file is saved as an artifact so it can be used in later stages like ECS deployment.
			
	2. Push the repo to Git main branch
	
	3. Create an IAM Role for CodeBuild.
		• AWS services like CodeBuild need permissions to interact with ECR (Elastic Container Registry), ECS, S3, and CodeBuild itself. To give CodeBuild all the necessary permissions, create new IAM Role
		• Might need to add inline policy for ECS to limit access instead of using managed policies which are broad
		
	4. Create a S3 Bucket for Build Artifacts
		• CodeBuild generates output files, such as Docker images and logs. We store them in Amazon S3 so they can be accessed later
		• ImageDefinition.json is the build artifact created after Build phase and is stored in S3 -> used for ECS deployment
		
	5. Create a CodeBuild Project.
		• The CodeBuild project is where AWS pulls your GitHub repository, builds the project (e.g., compiles code, builds Docker images), and pushes the results to S3.
		• Connect to Github here first
		• Configure Github repo, IAM Role, S3 here
		
	6. Test the CodeBuild Project.
		• Start the Build and check 'Build Logs'
		• Check artifacts created in S3 (imagedefintions.json is the artifact created here)
		
	7. Create an AWS CodePipeline
		• AWS CodePipeline is the service that connects all the different stages of your CI/CD pipeline. First, we need to create a new pipeline to define how changes in your code will trigger builds and deployments
		• Here configure, Source, Test and Deploy properties
		
	8. Test the Pipeline
		• Make the change in code and push to Git (here added Srikar to the title in index.html)
		• Verify the logs and pipeline
		NOTE: THIS RUNS NEW TASK IN ECS CLUSTER & IP OF WEBSITE ALSO CHANGES. IN PROD ATTACH THIS TO ALB
<img width="1372" height="2798" alt="image" src="https://github.com/user-attachments/assets/c30cddfd-7dac-47a8-bac8-614584e6cd32" />

Before change
  <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/68d8469d-3150-432e-af66-eaa6f7354271" />

  After change
    <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/45cc2e37-2ed8-4cf2-92f7-2b7731254fdf" />


