**AWS Continuous Integration project**

**Set Up GitHub Repository:**
The first step is to set up a GitHub repository to store our Python application's source code.

**Configure AWS CodeBuild:**
In this step, we'll configure AWS CodeBuild to build our Python application based on the specifications we define. CodeBuild will take care of building and packaging our application for deployment. Follow these steps ->
1. Log in to the AWS Console.
2. Open CodeBuild and create a new project.
3. Provide a project name and description, and select GitHub as the source.
4. Connect to GitHub using OAuth and create a secret with a custom name when prompted.
5. Create a role or select an existing one (you can attach policies to the role afterwards).
6. Write a Buildspec file in YAML format.
7. Use the Parameter Store from Systems Manager to store your Docker username, password, and URL. Reference these variables in the env section of the Buildspec file.
8. Create the CodeBuild project.
When you run the build, it may fail because the role does not have permissions for Systems Manager and Docker. Attach the necessary policies for these permissions. Additionally, edit the build to provide privileged access for Docker push.

**Create an AWS CodePipeline:**
In this step, we'll create an AWS CodePipeline to automate the continuous integration process for our Python application. AWS CodePipeline will orchestrate the flow of changes from our GitHub repository to the deployment of our application. Follow these steps ->
1. Log in to the AWS Console, open CodePipeline, and click on "Create Pipeline."
2. Create a new role as prompted and click "Next."
3. Select GitHub v2 as the source provider, connect to GitHub, provide the repository name, and specify the main branch name.
4. In the trigger configuration, do not select any filters.
5. Click "Next" and select CodeBuild as the build provider, using the project created in the previous step, then click "Next."
6. Do not select a deployment provider and click "Next."
7. Click "Create Pipeline."
The build will start automatically. Now, if you change anything in your GitHub repository and push, the pipeline will trigger a new build automatically.

**Trigger the CI Process:**
In this final step, we'll trigger the CI process by making a change to our GitHub repository. Let's see how it works ->
1. Go to your GitHub repository and make a change to your Python application's source code. It could be a bug fix, a new feature, or any other change you want to introduce.
2. Commit and push your changes to the branch configured in your AWS CodePipeline.
3. Head over to the AWS CodePipeline console and navigate to your pipeline.
4. You should see the pipeline automatically kick off as soon as it detects the changes in your repository.
5. It will fetch the latest code, trigger the build process with AWS CodeBuild, and deploy the application if you configured the deployment stage.

**CodeDeploy Steps:**
1. Go to CodeDeploy and create an application.
2. Create an EC2 instance and install the CodeDeploy agent on it. You can follow the installation guide here -> 
https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html
3. Create a role to grant the EC2 instance permission to communicate with CodeDeploy. Assign the CodeDeploy full access policy to this role.
4. Attach the role to the EC2 instance and restart the CodeDeploy agent service on the instance.
5. Similarly, create a role for CodeDeploy with full EC2 instance access.
6. In CodeDeploy, create a deployment group and attach the previously created role. Ensure to provide the Git repository and the latest commit ID. The appspec.yml and the scripts folder referenced in appspec.yml should be present at the root level of the repository.

**Final Steps:**
Go to CodePipeline, click on Edit, then Add Stage, and add a CodeDeploy stage. Select AWS CodeDeploy as the provider, and enter the application name and deployment group name created earlier.

**Complete Flow Overview:**
When a push occurs in GitHub, the source stage in CodePipeline will pull the latest code. CodeBuild will then be triggered with this latest code. After a successful build, the Docker image will be pushed to Docker Hub, and the deployment stage will be triggered, using the pushed Docker image to deploy it on the server.
