###Remote State, Remote Backends with Terraform Cloud and State commands

In Terraform, the state file is crucial as it maps Terraform configurations to real-world infrastructure. By default, this state file is stored locally in the terraform.tfstate file. While local state storage is convenient for individual projects, it doesn't scale well for team collaboration due to issues like state file corruption, lack of state locking, and the risk of exposing sensitive information. To address these challenges, Terraform allows you to store state remotely using remote backends, like S3 or Terraform Cloud, which offer secure, shared storage and state locking.

In this lab, we'll explore how to configure and use Terraform Cloud as a remote backend for your Terraform state file. You'll learn how to securely manage your infrastructure state, enabling collaboration while protecting sensitive data.


###Prerequisites

Before starting this lab, ensure you have the following:

--AWS Configuration: Use aws configure command to configure AWS.
--Terraform Cloud Account: Set up a free Terraform Cloud account.
--Create organization and workspace: In terraform cloud account create an organization and workspace if you don't have any.

###Task

You are part of a DevOps team tasked with managing the infrastructure for a new application. The application requires a VPC with a subnet. You need to configure Terraform Cloud as the remote backend for managing this infrastructure. The state file should be stored securely, with the ability to lock the state during updates to avoid conflicts. The configuration should be managed in a way that allows other team members to collaborate without risking state file corruption or exposing sensitive information.

##Solution and Steps

Solution and Steps:

Step 1: Log in to Terraform Cloud
Generate a Terraform Cloud Token:

Go to your Terraform Cloud account and generate an API token.
Copy the token to use in the next step.

Log in to Terraform Cloud:
--terraform login


Step 2: Initialize a New Terraform Configuration
Create a Directory for Your Project:

Create the Terraform Configuration Files:

main.tf (AWS provider and resource definitions)

terraform.tf (Terraform Cloud backend configuration)


Step 3: Initialize the Terraform Project

Initialize the Terraform Project:

--terraform init

This will configure Terraform to use the remote backend with Terraform Cloud.


Step 4: Set the execution mode

Go to terraform cloud:

Navigate to your workspace.
Go to workspce settings > general.
Set the execution mode to Local and save it.

Step 5: Plan and Apply the Configuration
Run Terraform Plan
--terraform plan

Review the execution plan to ensure everything is set up correctly.

Run Terraform Apply

--terraform apply

Confirm the apply to create the infrastructure.

Step 6: Verify Remote State Storage
Log in to Terraform Cloud:

Navigate to your workspace and verify that the state file is stored remotely.

Working with Terraform State Commands
After deploying your infrastructure, managing and interacting with the Terraform state is crucial. Terraform state commands allow you to view, modify, and manipulate the state file directly. Below are some essential commands with their explanations:

Viewing and Managing Terraform State
1. Listing Resources in the State
--terraform state list

This command lists all the resources tracked in the current state file. It's useful to verify that all the expected resources are being managed by Terraform.

2. Showing Detailed Information About a Resource
--terraform state show <resource_id>

This command shows detailed information about a specific resource from the state file. Replace <resource_id> with the actual resource ID (e.g., aws_vpc.main_vpc), which you can get from the terraform state list command.

3. Moving Resources Between Modules or Namespaces

--terraform state mv <source> <destination>

The mv command is used to move a resource from one location to another within the state file. This can be helpful if you refactor your configuration and need to reorganize resources without recreating them.

Example:

--terraform state mv aws_subnet.main_subnet aws_subnet.my_main_subnet

This example moves/changes name of a subnet , which is useful when we want to change without recreating it again. Now we need to change the name of the subnet manually.

If we run terraform apply no resource will be created.

4. Removing Resources from the State

--terraform state rm <resource_id>

This command removes a resource from the Terraform state file without destroying the resource in AWS. This is useful if you want Terraform to stop managing a particular resource.

Example:

--terraform state rm aws_subnet.my_main_subnet


This will remove the subnet without destrying.

5. Pulling the Latest State

--terraform state pull

This command pulls the latest state from the remote backend (e.g., Terraform Cloud) and displays it locally. It's useful when you want to inspect the current state without making changes.

These commands help you manage your Terraform state effectively, allowing for better control and organization of your infrastructure.

Conclusion

In this lab, you have successfully set up Terraform Cloud as a remote backend for managing the state of your Terraform project. By storing the state file in Terraform Cloud, you've ensured that the infrastructure is managed securely and efficiently, with features like state locking and team collaboration capabilities. This setup allows you to manage infrastructure in a more scalable and secure manner, making it ideal for production environments and team projects. This lab also includes the necessary state commands.
