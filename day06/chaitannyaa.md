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

- Provider configuration and authentication mechanisms in Terraform.

To configure a provider, we must specify its name and the authentication credentials for the platform it represents. There are several authentication mechanisms for each provider, which we must set up before we can use them. The following examples use AWS, but the mechanisms and processes for other providers are similar.

**Configuring Providers**

To configure a provider, we declare a provider block in our Terraform configuration file. Below is an example provider block for AWS on us-east-1 region:

```sh
provider "aws" {
  region = "us-east-1"
}
```

This block declares a provider named aws and sets its region to us-east-1.

**Authentication Mechanisms for AWS**

Here are the authentication mechanisms that we can use to authenticate on AWS:

Access keys are access and secret keys which can be used to access and manage AWS resources. It is recommended to use environment variables or configuration files rather than hardcoding the access keys in the provider block.

1. Set environment variables

```sh
export AWS_ACCESS_KEY_ID="access-key"
export AWS_SECRET_ACCESS_KEY="secret-key"
```

In the provider block, we can reference the environment variables:

```sh
provider "aws" {
  region = "us-east-1"
  access_key = "${var.AWS_ACCESS_KEY_ID}"
  secret_key = "${var.AWS_SECRET_ACCESS_KEY}"
}
```

2. Use configuration files:

AWS CLI configuration files are located at ~/.aws/credentials. We can reference the access keys in our provider block by specifying the path to the file:

```sh
provider "aws" {
  region = "us-east-1"
  shared_credentials_file = "/path/to/aws/credentials/file"
  profile = "profile-name"
}
```
**Authentication Mechanisms for Azure**

Similar to AWS, Azure also has various authentication mechanisms to authenticate Terraform. Here are the authentication mechanisms that we can use to authenticate on Azure:

1. Azure AD Service Principal:

Azure Active Directory (AAD) service principals are a way to authenticate applications and services so they can access Azure resources. When creating a service principal, we must create a client ID and secret.

To authenticate Terraform with Azure AD service principal, we need to specify the client_id and client_secret in the provider block. Here's an example of how to declare an Azure provider using a service principal:

```sh
provider "azurerm" {
  subscription_id = "12345678-1234-1234-1234-1234567890ab"
  client_id       = "87654321-4321-4321-4321-ba0987654321"
  client_secret   = "my-client-secret"
  tenant_id       = "11111111-1111-1111-1111-111111111111"
}
```

2. Managed Service Identity (MSI):

Managed service identity (MSI) is a feature enabled on Azure services that provides an automatically managed identity for the resources deployed and access to Azure resources. When using an MSI-enabled resource, we can authenticate to Azure resources without the need for managing secrets.

To authenticate Terraform with Azure MSI, we need to specify the identity attribute in the provider block. Here's an example of how to declare an Azure provider using MSI:

```sh
provider "azurerm" {
  subscription_id = "12345678-1234-1234-1234-1234567890ab"
  identity = {
    type = "systemAssigned"
  }
}
```
3. Azure CLI:

Terraform can use the credentials stored in the Azure CLI to authenticate to Azure resources. To use the Azure CLI, we only need to declare the provider block without any authentication credentials.

Here's an example of how to declare an Azure provider that uses the Azure CLI for authentication:

```sh
provider "azurerm" {}
```

**Authentication Mechanisms for GCP**

1. Using Service Account Key file for Authentication:

```sh
provider "google" {
  credentials = file("/path/to/your/service-account-key.json")
  project     = "my-project-id"
  region      = "us-central1"
}
```

In this approach, you can authenticate using a service account key file, which you can create or obtain from the Google Cloud Console. Once you have the key file, you can configure the Google Cloud Platform provider to use that file using the credentials parameter, with the file path and name as the value.

2. Using Application Default Credentials (ADC) for Authentication:

```sh
provider "google" {
  credentials = "application_default"
  project     = "my-project-id"
  region      = "us-central1"
}
```

In this approach, the GCP provider uses the application_default_credentials method to authenticate the Terraform script with Google Cloud. This method uses your local application default credentials, which are obtained using the same authentication flow as the Google Cloud SDK and other Google Cloud client libraries.

3. Using Environment Variables for Authentication:

```sh
provider "google" {
  credentials = "env:GOOGLE_APPLICATION_CREDENTIALS"
  project     = "my-project-id"
  region      = "us-central1"
}
```

In this approach, you can authenticate using environment variables that specify the path to a service account key file. The credentials parameter value is set to env:GOOGLE_APPLICATION_CREDENTIALS, and the path to the service account key file is set in the environment variable GOOGLE_APPLICATION_CREDENTIALS.

4. Using the Metadata Server for Authentication (Google Compute Engine VM Only):

```sh
provider "google" {
  credentials = "metadata"
  project     = "my-project-id"
  region      = "us-central1"
}
```

In this approach, you can authenticate using the instance's built-in metadata server. With this method, instances can authenticate themselves and access resources on behalf of the service account attached to that instance.

- Set up authentication for each provider on your local machine to establish the necessary credentials for interaction with the respective cloud platforms.

**Set up the authentication for AWS, Azure, and GCP--->**

AWS:

- Create an AWS account if you do not have one already.

https://repost.aws/knowledge-center/create-and-activate-aws-account

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/6cfa939e-d78d-4ec2-b594-888aa7f22ed4)

- Create an IAM user with programmatic access and attach the necessary permissions to access and manipulate your AWS resources.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/4a15fbaa-d2f0-471f-8973-33ada6833cc3)

- Create an access key and secret key for the IAM user and note them down.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/93b62e87-152d-4a9f-809e-f6ed6ecdeb07)

- Export the AWS access key and secret key as environment variables.

**For Linux and macOS, you can do this by executing the following commands, replacing the values with your access key and secret key:**

```sh
export AWS_ACCESS_KEY_ID="your_aws_access_key"
export AWS_SECRET_ACCESS_KEY="your_aws_secret_key"
```

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/c090cb77-29f0-49b3-b52c-62cbd2994ffb)

**For Windows, you can do this by executing the following commands, replacing the values with your access key and secret key:**

```sh
setx AWS_ACCESS_KEY_ID YOUR_ACCESS_KEY_ID
setx AWS_SECRET_ACCESS_KEY YOUR_SECRET_ACCESS_KEY
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/9bbb1ed6-5dc0-47cd-b15f-5737750351e5)

Azure:

- Create an Azure account if you do not have one already.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/93db23cd-782c-4b26-9cf5-d23368f3c39a)

- Create a service principal with the necessary permissions to access and manipulate your Azure resources.

- Note down the Azure subscription ID, client ID (also known as application ID), client secret (also known as password), and tenant ID.

- Export the Azure subscription ID, client ID, client secret, and tenant ID as environment variables. For Linux and macOS, you can do this by adding the following lines to your .bashrc or .bash_profile file, replacing the values with your own subscription ID, client ID, client secret, and tenant ID:

```sh
export ARM_SUBSCRIPTION_ID="your_subscription_id"
export ARM_CLIENT_ID="your_client_id"
export ARM_CLIENT_SECRET="your_client_secret"
export ARM_TENANT_ID="your_tenant_id"
```

GCP:

- Create a GCP account if you do not have one already.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/4f2525a1-3f94-4320-bc7f-9b7f135baaba)

- Set up the appropriate permissions to access and manipulate your GCP resources.

- Create a new service account key and note down the downloaded key file's path.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/e7591571-d2f6-471a-9772-488f8e958416)

- Export the GCP service account key as an environment variable. For Linux and macOS, you can do this by running the following command, replacing the path/to/service-account-key.json with the actual path and filename of your service account key file:

```sh
export GOOGLE_CREDENTIALS=$(cat path/to/service-account-key.json)
```

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/1d35efac-5b9e-4c0c-873e-fbe32bd4d766)

Or

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/26e784c9-0aef-496e-899f-4499df0ddbb4)

**With your environment variables set up, you can now use Terraform to provision and manage your AWS, Azure, or GCP resources.**

## Gain hands-on experience using Terraform providers for your chosen cloud platform.

- Choose a cloud platform (AWS, Azure, Google Cloud, or others) as your target provider for this task.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/27e82d8a-248e-4a29-8a51-9bc6f70889f0)

- Create a Terraform configuration file named `main.tf` and configure the chosen provider within it.

**Configuration file to create a aws_security_group**

```sh
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }
  required_version = "~> 1.3.9"
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_security_group" "main" {
  egress = [
    {
      cidr_blocks      = [ "0.0.0.0/0" ]
      description      = ""
      from_port        = 0
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "-1"
      security_groups  = []
      self             = false
      to_port          = 0
    }
  ]
  ingress = [
    {
      cidr_blocks      = ["0.0.0.0/0"]
      description      = ""
      from_port        = 22
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "tcp"
      security_groups  = []
      self             = false
      to_port          = 22
    },
    {
      cidr_blocks      = ["0.0.0.0/0"]
      description      = ""
      from_port        = 80
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "tcp"
      security_groups  = []
      self             = false
      to_port          = 80
    }
  ]

  tags = { Name = "Terraform_Generated" }
}
```  
- Authenticate with the chosen cloud platform using the appropriate authentication method (e.g., access keys, service principals, or application default credentials).

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/9f2ec5c0-5ac9-4315-8e5b-400961ada0cc)

- Deploy a simple resource using the chosen provider. For example, if using AWS, you could provision aws_security_group.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/7bd2d1c3-0204-4011-804b-57216453f58c)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/a6c26c59-2344-416a-a3a5-19d152abd482)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/d7362092-5b16-4077-860d-1f31acbfe8c1)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/d2b4caf2-f9b6-4389-ba77-d53d3943816d)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/1dc65671-1d4f-453b-a5ae-45c8675030e0)

- Experiment with updating the resource configuration in your `main.tf` file and apply the changes using Terraform. Observe how Terraform intelligently manages the resource changes.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/632ed873-9aa4-4074-b31b-9e99fbb51aa2)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/a8caf79b-c3d8-4ea2-b723-0e878ec78251)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/35313d20-1733-43a9-ac65-6911529b7ea8)

- Once you are done experimenting, use the `terraform destroy` command to clean up and remove the created resources.

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/5b9d07ee-ceb8-4f9f-aa34-bf243cd079a1)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/6f525286-ccdb-4772-ad9e-9feab6eb238c)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/34519860-1f8c-4438-baf6-1b0145bd8e27)

### Happy Learning! 🌍💻
