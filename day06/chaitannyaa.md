# TerraWeek Day 6: Terraform Providers

## Learn about Terraform providers and compare their features across different cloud platforms.

- Terraform providers and their significance in managing resources across various cloud platforms or infrastructure services.

Terraform Providers are plugins that enable interaction between Terraform tools and external API services for cloud infrastructure management. They define the set of resources and APIs supported by a cloud platform. Terraform Providers are essential for managing infrastructure for different cloud platforms like AWS, Azure, Google Cloud, and many more..

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/77e71b8b-584e-451b-82ac-8a2131c3075e)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/d6123518-4db7-47e9-aad3-65dddc347b36)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/82d0d218-e403-4983-b8a6-2d8388cd73e5)

**There are almost 3268 terraform providers available as of now including providers from official, partner and community.**

- Compare the features and supported resources for each cloud platform's Terraform provider to gain a better understanding of their capabilities.

Some key features of Terraform Providers:

- **They enable infrastructure configuration for cloud platforms**

- **They simplify the process of creating, updating, and deleting resources on cloud platforms**

- **They provide a unified language for managing different cloud platforms**

- **They allow for the use of third-party Terraform providers**

Each cloud provider's Terraform Provider has its set of resources and features. 
Let's compare features for AWS, Azure, and Google Cloud Providers.

| AWS | Azure | GCP |
| -------------- | -------------- | -------------- |
| Supports up to 512 resources and data sources | Supports over 500 resources and data sources | Supports over 100 Google Cloud Platform (GCP) services and resources |
| Supports various AWS services like EC2, VPC, ELB, S3, and RDS | Supports various Azure services like Virtual Machines (VMs), Virtual Networks (VNets), SaaS applications, and more. | Supports GCP services like Compute Engine, Kubernetes Engine, IAM, Pub/Sub, and more |
| Supports different authentication methods like static credentials, environment variables, and EC2 instance profiles | Supports different authentication methods like Azure CLI, Service Principal, Managed Identity, and environment variables | Supports authentication using Google Cloud default credentials, service account JSON file |
| Includes AWS-specific features like IAM role and policy management, VPC peering, Route 53 DNS configuration, SNS topic creation, and CloudFormation stack import | Offers integration with various Azure services, such as Azure Container Instances, Azure Kubernetes Service (AKS), Azure Functions, Azure Active Directory (AAD), Azure Monitor, and Azure Policy | Supports creating and managing IAM roles and service accounts in Google Cloud, allowing you to control access and permissions for resources |
| Has a large and active community, providing numerous community-maintained modules and resources for managing AWS infrastructure using Terraform | Support for deploying and managing Azure Managed Applications, allowing you to package and distribute applications as self-contained Terraform modules | Compatible with Google Cloud Deployment Manager, allowing you to import existing Deployment Manager templates or migrate from Deployment Manager to Terraform |

## Explore provider configuration and set up authentication for each provider.

- Explore provider configuration and authentication mechanisms in Terraform.


- Set up authentication for each provider on your local machine to establish the necessary credentials for interaction with the respective cloud platforms.


## Gain hands-on experience using Terraform providers for your chosen cloud platform.

- Choose a cloud platform (AWS, Azure, Google Cloud, or others) as your target provider for this task.

  
- Create a Terraform configuration file named `main.tf` and configure the chosen provider within it.

  
- Authenticate with the chosen cloud platform using the appropriate authentication method (e.g., access keys, service principals, or application default credentials).


- Deploy a simple resource using the chosen provider. For example, if using AWS, you could provision a Virtual Private Cloud (VPC), Subnet Group, Route Table, Internet Gateway, or a virtual machine.


- Experiment with updating the resource configuration in your `main.tf` file and apply the changes using Terraform. Observe how Terraform intelligently manages the resource changes.


- Once you are done experimenting, use the `terraform destroy` command to clean up and remove the created resources.


### Happy Learning! 🌍💻
