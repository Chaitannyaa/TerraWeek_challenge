# TerraWeek Day 4/7

Terraform is a popular Infrastructure as Code (IaC) tool that is used to provision and manage cloud resources across different cloud providers such as AWS and Azure. One of the key aspects of Terraform is its state management feature, which helps track the current state of resources and ensures smooth infrastructure provisioning and management. In this blog post, we will dive into the importance of Terraform state, different methods of storing state files, and how to leverage remote state management.

## Importance of Terraform State

Terraform state is a critical aspect of managing infrastructure using Terraform. Terraform's state file is a JSON-formatted file that contains all the information about the created resources, including variables, configurations, dependencies, and relationships between resources. It's a record of the state of the infrastructure that Terraform manages, and it's essential for resource management and tracking.

The Terraform state file is used to determine what infrastructure resources to create, delete, or modify. It also keeps track of configuration changes, such as updates to resource properties or dependencies. The Terraform state file is critical in managing infrastructure as it:

- Tracks changes in infrastructure over time
- Ensures the actual infrastructure state matches the desired state
- Helps maintain consistency by preventing changes that conflict with existing resources

## Local State and `terraform state` Command

By default, Terraform stores the state file locally on the machine running the Terraform command. This method is called local state storage and is suitable for small and simple infrastructures. Managing state files manually can lead to errors and data loss, which is why Terraform provides the terraform state command to manage the state file, including show, list, and remove commands.

**Local State Storage**

To enable local state storage, you don't need to do anything special. This is the default behavior of Terraform. When you run a command like terraform apply, Terraform will automatically create a state file named terraform.tfstate in the same directory as your configuration file.

**Managing State with terraform state command**

You can use the terraform state command to view, modify, and delete resources within your state file. Here are some examples:

- To list all the resources in your state file, run:

```sh
terraform state list
```

- To view the current state of a specific resource, run:

```sh
terraform state show [resource-name]
```
Replace 'resource-name' with the name of the resource you want to show the current state of.

- To remove a resource from the state file, run:

```sh
terraform state rm <resource-name>
```

This will remove the specified resource and its associated state from the state file. Use this command with caution - removing a resource from the state file can cause issues during future operations.

## Remote State Management

With remote state management, you store the state file remotely, allowing multiple team members to work together on the same infrastructure code without affecting each other. There are various options for remote state management, including Terraform Cloud, AWS S3, Azure Storage Account, or HashiCorp Consul.

When using remote state management, the Terraform state is stored in a central location that can be accessed by all team members involved in the project. This helps eliminate issues associated with local state management, such as conflicting changes, version control issues, and the risk of losing the state file.

**To store the state remotely in an S3 bucket using the backend configuration block:**

```sh
# backend.tf

terraform {
  backend "s3" {
    bucket = "my-terraform-state-bucket"
    key    = "terraform.tfstate"
    region = "us-west-2"
  }
}
```

And now, here is the complete step-by-step guide to using remote state management with Terraform and AWS S3.

Step 1: Create an S3 bucket

The first step to using S3 as the remote state backend is to create an S3 bucket in your AWS account. You can create the bucket through the AWS Management Console or using the resource in your terraform configuration file. Keep in mind that the bucket name must be globally unique across all of AWS, so choose a unique name. You should also consider enabling versioning on the bucket to avoid losing any previous versions of your Terraform state.

Step 2: Modify your Terraform configuration file

Once you have an S3 bucket, you can modify your Terraform configuration file to use S3 as the remote state backend. You'll need to add the backend configuration block to your configuration file. Here's an example:

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

resource "local_file" "DevOps" {
        filename = "C:/Users/Chait/Desktop/Terrform/remote_state_management/terra_generated.txt"
        content = "I am a DevOps engineer, who knows terraform very well"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "chaitannyaa-terraform-state-bucket"
  lifecycle {
    prevent_destroy = true
  }
}

resource "aws_s3_bucket_versioning" "my_bucket_versioning" {
  bucket = aws_s3_bucket.my_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
```
This configuration tells Terraform to use the S3 bucket my-terraform-state-bucket to store the state file. The key parameter specifies the filename for the state file within the bucket. Finally, the region parameter specifies the AWS region where the bucket is located.

Step 3: Initialize the backend

After modifying your Terraform configuration file, you can initialize the S3 backend by running the terraform init command. This will download the necessary provider plugins and create the remote state bucket in S3.

```sh
terraform init
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/1a187c06-955c-468f-a6ba-71ca0899e3d2)

Step 4: Use your configuration file to manage resources

Now that you have set up the remote state backend, you can use your Terraform configuration file to manage resources. When you run terraform apply, Terraform will automatically store the state file in the S3 bucket you specified.

```sh
terraform apply
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/67df6b2d-8d99-42ea-af1f-43b7bfd60fdc)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/e5df98f0-84d1-4733-a413-9987f4f529f8)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/b0a074ae-adbd-47d4-9f68-67e045543a57)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/7159d963-2c41-495d-995e-f6d5c14b4456)

Step 5: Access your state file

If you need to access your state file for any reason, you can use the terraform state command. For example, to list all the resources in your state file, run:

```sh
terraform state list
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/3238b300-b93b-473a-9b84-88150e38967f)

## Remote State Configuration

📚 **Modify**: Enhance your Terraform configuration file to store the state remotely using the chosen remote state management option. Include the necessary backend configuration block in your Terraform configuration file to enable seamless remote state storage and access.

```hcl
terraform {
  backend "<chosen_backend>" {
    # Add required configuration options for the chosen backend
  }
}
```
### Let's add backend S3 block to use it for remote state files storage--->

```hcl
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

resource "local_file" "DevOps" {
        filename = "C:/Users/Chait/Desktop/Terrform/remote_state_management/terra_generated.txt"
        content = "I am a DevOps engineer, who knows terraform very well"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "chaitannyaa-terraform-state-bucket"
  lifecycle {
    prevent_destroy = true
  }
}

resource "aws_s3_bucket_versioning" "my_bucket_versioning" {
  bucket = aws_s3_bucket.my_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}

terraform {
  backend "s3" {
    bucket = "chaitannyaa-terraform-state-bucket"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
```

```sh
terraform init
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/0a1e3553-72f2-4800-a50c-e4b51353d916)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/4b035dc5-4cd5-4848-afa2-29022282cdd6)

```sh
terraform apply
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/22366cbb-6b50-4a9e-af1a-b9f71bf58731)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/6b56aa62-851e-4f73-b2f9-727521f94ac2)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/25f4948c-085b-4990-af5c-024e69b9ec95)

### Now do change your terraform configuration to operate changes to it--->

```sh
terraform apply
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/e80b13f7-56a9-440c-81a0-c9072024b086)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/ff9eb099-cc95-4b23-94a3-32fed4a034d9)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/de99e564-e409-4f49-969c-c1dc446776bf)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/d0b864a8-87c5-4523-987f-169eb05fe920)

I hope you got the use of remote state storage management using AWS S3.
Happy Learning :)

