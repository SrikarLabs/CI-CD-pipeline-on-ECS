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


<img width="1372" height="788" alt="image" src="https://github.com/user-attachments/assets/5579b8ba-c742-482c-b8f5-a96c9002d6e7" />


<img width="1372" height="2798" alt="image" src="https://github.com/user-attachments/assets/c30cddfd-7dac-47a8-bac8-614584e6cd32" />

Before change
  <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/68d8469d-3150-432e-af66-eaa6f7354271" />

  After change
    <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/45cc2e37-2ed8-4cf2-92f7-2b7731254fdf" />


