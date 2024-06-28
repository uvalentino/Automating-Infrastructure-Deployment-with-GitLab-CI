# Automating-Infrastructure-Deployment-with-GitLab-CI


![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)


## Project Overview
For this project, we have two main modules: one for configuring a Virtual Private Cloud (VPC), including a subnet, internet gateway, and security group, and another for deploying an EC2 instance within the VPC. The VPC module ensures a secure and isolated network environment for our resources, while the EC2 module provides a scalable compute instance.
The project is divided into two parts:
1. Write Terraform code and manually create the infrastructure by running Terraform commands.
2. Create a CI/CD pipeline to automate the creation of Terraform resources.

### PART 1: MANUAL INFRASTRUCTURE PROVISIONING
**Setting Up the Environment**

To get started, ensure you have the necessary tools installed:
Terraform: Download and install Terraform from the official website or use a package manager.
AWS CLI: Install the AWS Command Line Interface to interact with AWS services.
GitLab Account: Sign up for a GitLab account if you haven't already. Created and downloaded the HTTPS token key for your Gitlab account.
Before diving into CI/CD setup, let's ensure we have a backend for storing the Terraform state. We'll use an S3 bucket for this purpose:
1. Create an S3 bucket and enable versioning, keeping all other settings at their default values.
2. Create a DynamoDB table with an attribute named "LockID", leaving all other settings as default.

**Create the following files to set up our infrastructure**

- Create a folder named automation and open it in VS Code. Then, create the following files.
backend.tf

Make sure to replace the placeholders with your actual values:

- **your_bucket_name:** The name of your S3 bucket.

- **your_aws_region:** The AWS region where your S3 bucket and DynamoDB table are located.

- **your_dynamodb_table_name:** The name of your DynamoDB table used for state locking.

This configuration sets up Terraform to use an S3 bucket for storing the state file and a DynamoDB table for state locking to prevent concurrent operations.

- Create a folder named **vpc** to serve as the first module. Inside this folder, create the following files: main.tf, outputs.tf, and variables.tf.

- Create a folder named **web** to serve as the second module. Inside this folder, create the following files: main.tf, outputs.tf, and variables.tf.

After creating all the files, deploy your infrastructure to AWS using the following sequence of commands:
1. Initialize Terraform: terraform init
2. Validate Terraform configuration: terraform validate
3. Generate and review an execution plan: terraform plan
4. Apply the execution plan to create resources in AWS: terraform apply
Once the resources have been successfully created in AWS, you can proceed to clean up by running: terraform destroy
After deleting all the created resources, you can proceed to the automation part.

### PART2: INFRASTRUCTURE AUTOMATION WITH CICD PIPELINE 

**Create GitLab repository**

Create GitLab repository
Here are the steps to create a new public GitLab repository named "auto":
1. Sign in to GitLab: Go to [GitLab](https://gitlab.com/) and sign in to your account.
2. Go to Projects: Once logged in, navigate to the "Projects" section.
3. Create New Project: Click on the "New project" button.
4. Choose Project Visibility: In the "Create a new project" page, select the "Public" option for the project visibility.
5. Set Project Name: Enter "auto" as the project name.
6. Configure Project: You can configure other settings such as description, project URL, tags, etc., if desired.
7. Configure Project: You can configure other settings such as description, project URL, tags, etc., if desired.
8. Create Project: Click on the "Create project" button to create the repository.
9. Repository Setup: Once the repository is created, you'll be redirected to the repository page where you can find the repository URL, clone command, and other options.
Now, you have successfully created a new public GitLab repository named "auto".

**Push your code to GitLab**

1. Before pushing your code, create a **.gitignore** file and paste the following content into it. This will ensure that only the necessary files are pushed to GitLab.
2. Execute the following command to clone your project repository:
   ```bash
    git clone https://<link-to-your-project-repository>
    ```
3. Copy the files into your cloned repository.
Execute the following commands:

 - To create a new branch named dev: git checkout -b dev
 - Stage all changes: git add .
 - Commit the changes with a message: git commit -m "Initial commit"
 - Push the dev branch to the remote repository: git push -u origin dev
4. Create a merge request: In GitLab, navigate to your repository and create a merge request from the dev branch to main.

**Configuring GitLab CI/CD Pipeline**

Now, let's automate our Terraform deployments using GitLab CI/CD:

- Create a .gitlab-ci.yml file: Define your CI/CD pipeline stages, jobs, and configuration.

**Running the Pipeline**

With your CI/CD configuration in place, GitLab will automatically trigger the pipeline whenever changes are pushed to the mainbranch. The pipeline will execute Terraform commands to plan and apply the infrastructure changes defined in your modules.

**Conclusion**
In this project, we demonstrated how to automate Terraform deployments using a GitLab CI/CD pipeline. By combining infrastructure as code practices with CI/CD automation, teams can effectively manage and scale their cloud infrastructure, ensuring both consistency and reliability.
Feel free to explore additional customization options and integrations to tailor the pipeline to your specific needs and workflows. Happy automating!
