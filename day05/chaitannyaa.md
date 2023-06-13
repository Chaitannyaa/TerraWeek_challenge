# TerraWeek Day 5

## Task 1: 
- What are **modules** in Terraform and why do we need modules in Terraform?

Modules in Terraform are self-contained packages of Terraform configuration that are used to organize and reuse infrastructure code. At its simplest, a module can be a collection of resources that are used together to create a particular piece of infrastructure, such as an EC2 instance in AWS, a Resource Group in Azure, or a Cloud Storage bucket in GCP.

Here's an example of how you might use a simple module to create an EC2 instance on AWS:

1. First, you would create a directory to hold your module. In this example, I'll call it ec2-instance:

```sh
./modules/ec2-instance/
```

2. Inside this directory, you would create xyz.tf file to hold your Terraform code. Let's call it main.tf. Here's some sample code that defines an EC2 instance:

```sh
# ./modules/ec2-instance/main.tf

resource "aws_instance" "ec2-instance" {
  ami           = var.ami
  instance_type = var.instance_type
  key_name      = var.key_name

  tags = {
    Name = "example-instance"
  }
}
```

This code defines an EC2 instance resource in AWS using AWS provider. The ami, instance_type, and key_name are variables that can be passed into the module when it is used.

3. Next, you would create a file to hold input variables for your module. Let's call it variables.tf:

```sh
# ./modules/ec2-instance/variables.tf

variable "ami" {
  default = "ami-0c55b159cbfafe1f0"
}

variable "instance_type" {
  default = "t2.micro"
}

variable "key_name" {
  default = ""
}
```

This file defines the variables that can be passed into our ec2-instance module.

4. Finally, you would create a file to hold output variables for your module. Let's call it outputs.tf:

```sh
# ./modules/ec2-instance/outputs.tf

output "public_ip" {
  value = aws_instance.ec2-instance.public_ip
}

output "instance_id" {
  value = aws_instance.ec2-instance.id
}
```

This file defines the output values that our ec2-instance module will return when it is used.

5. Once you've created your module, you can use it in your Terraform code by calling module.<name> and passing in any necessary inputs.

```sh
# main.tf

module "my_instance" {
  source = "./modules/ec2-instance"

  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  key_name      = "my-key"
}

output "public_ip" {
  value = module.my_instance.public_ip
}

output "instance_id" {
  value = module.my_instance.instance_id
}
```
  
In this example, we're using the ec2-instance module we defined earlier and passing in the necessary inputs (ami, instance_type, and key_name). The public_ip and instance_id outputs from the ec2-instance module are returned through the module.my_instance. syntax.

- What are the benefits of using modules in Terraform?

There are several benefits to using modules in Terraform:

**Reusability**: Modules make it easy to reuse infrastructure code across multiple projects or environments. This saves time and reduces errors.

**Simplicity**: Modules provide a simple, standardized way to organize infrastructure code, which makes it easier to read, write, and maintain.

**Scalability**: With modules, you can scale your infrastructure code to handle complex deployments and large teams.

**Abstraction**: Modules abstract away complex infrastructure details, allowing you to focus on the higher-level functionality of your application.
  
## Task 2: 
  
- Create/Define a module in Terraform to encapsulate reusable infrastructure configuration in a modular and scalable manner. For e.g. $\textcolor{yellow}{\textsf{EC2 instance in AWS}}$, $\textcolor{lightblue}{\textsf{Resource Group in Azure}}$, $\textcolor{red}{\textsf{Cloud Storage bucket in GCP}}$.
  
Let's create a module in Terraform to encapsulate the reusable infrastructure configuration for an EC2 instance in AWS, a Resource Group in Azure, and a Cloud Storage bucket in GCP.

Here's an example of how you can define the module:

```hcl
# /modules/infra/main.tf

variable "instance_name" {
  description = "Name for the EC2 instance"
  type        = string
}

variable "region" {
  description = "AWS region for the EC2 instance"
  type        = string
}

variable "resource_group_name" {
  description = "Name for the Azure Resource Group"
  type        = string
}

variable "storage_bucket_name" {
  description = "Name for the GCP Cloud Storage bucket"
  type        = string
}

variable "location" {
  description = "Location for the Azure Resource Group and GCP Cloud Storage bucket"
  type        = string
}

# AWS EC2 instance resource
resource "aws_instance" "example" {
  ami           = var.ami_id
  instance_type = var.instance_type
  subnet_id     = var.subnet_id
  tags = {
    Name = var.instance_name
  }
}

# Azure Resource Group resource
resource "azurerm_resource_group" "example" {
  name     = var.resource_group_name
  location = var.location
}

# GCP Cloud Storage bucket resource
resource "google_storage_bucket" "example" {
  name     = var.storage_bucket_name
  location = var.location
}
```
  
In this example, we have defined a module that consists of an EC2 instance in AWS, a Resource Group in Azure, and a Cloud Storage bucket in GCP. The module accepts input variables for customizing the configuration of each resource.

To use this module, create a separate directory for the module and place the above main.tf file inside it. Then, reference the module in your main Terraform configuration:

```hcl
# main.tf

module "infra_module" {
  source                  = "./modules/infra"
  instance_name           = "my-ec2-instance"
  region                  = "us-west-2"
  resource_group_name     = "my-resource-group"
  storage_bucket_name     = "my-storage-bucket"
  location                = "US"
}
```
  
In this example, we invoke the infra_module module and provide values for the input variables specific to each resource. Adjust the values as per your requirements.

By using this module, you can easily create an EC2 instance in AWS, a Resource Group in Azure, and a Cloud Storage bucket in GCP with consistent and reusable configurations. This approach promotes code organization and scalability in managing infrastructure resources across different cloud providers.

## Task 3: 
- Dig into **modular composition** and **module versioning**.
  
**Modular composition** refers to the ability to combine multiple modules together to create more complex infrastructure configurations. It allows you to build upon existing modules and compose them in a way that suits your specific requirements. With modular composition, you can create higher-level abstractions and assemble infrastructure components like building blocks.

Modular composition is achieved by leveraging the module block in Terraform. Here's an example of how you might combine two modules to create more complex infrastructure configurations:

```hcl
module "vpc" {
  source = "github.com/terraform-aws-modules/terraform-aws-vpc"

  name = "my-vpc"
  cidr = "10.0.0.0/16"
}

module "web" {
  source = "github.com/terraform-aws-modules/terraform-aws-web-server-cluster"

  name               = "web"
  security_group_ids = [module.vpc.security_group_id]
  subnet_ids         = module.vpc.private_subnet_ids
}
```

In this example, we're using the terraform-aws-vpc and terraform-aws-web-server-cluster modules to create a VPC and a web server cluster. The web module depends on the vpc module as it requires the VPC security group ID as well as the private subnet IDs.

**Module versioning** is an essential aspect of managing modules in Terraform. It allows you to track and control the versions of modules used in your infrastructure deployments. By specifying module versions, you ensure that deployments are predictable, reproducible, and resistant to breaking changes. Module versions also provide the ability to introduce new features or improvements without affecting existing deployments.

Terraform provides several approaches for versioning modules. Here are some examples:

### Unconstrained Versions
  
By default, Terraform uses unconstrained versioning, which means it will always fetch the latest version of a module when it is referenced. Here's an example of a module source configuration referencing a module using unconstrained versioning:

```sh
module "example" {
  source = "github.com/organization/module"
}
```

In this example, Terraform will always download the latest version of the module from the github.com/organization/module repository.

### Pinned Versions
You can also specify a specific version of a module to use by pinning it in the module source configuration. Here's an example of how to pin a module to version 1.2.3:

```sh
module "example" {
  source  = "github.com/organization/module"
  version = "1.2.3"
}
```
  
In this example, Terraform will always use version 1.2.3 of the module from the github.com/organization/module repository.

### Module Registry
If you publish your modules to a module registry, such as the Terraform Registry, you can reference them using the registry and version constraints. Here's an example of how to reference a module from the Terraform Registry:

```sh
module "example" {
  source = "terraform-aws-modules/ec2-instance/aws"
  version = "2.0.0"
}
```

In this example, Terraform is referencing the aws module for creating EC2 instances from the terraform-aws-modules organization in the Terraform Registry. The module is constrained to version 2.0.0.

## Task 4: 
  
- What are the ways to **lock Terraform module versions**? Explain with code snippets.

### lock Terraform module versions with .terraform.lock.hcl file
  
The .terraform.lock.hcl file is automatically created when you run terraform init command in a Terraform project directory. It is used to lock the versions of the providers and modules used in your Terraform configuration. It ensures that subsequent runs of terraform init and terraform apply use the same versions of the providers and modules, providing consistent behavior across different environments and team members.

It includes information about the provider versions and module sources that were resolved and used during the terraform init command.
  
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/a5105731-b050-422b-834e-3644d22a1f7c)

It's important to include the .terraform.lock.hcl file in version control systems (like Git) along with your Terraform configuration files (*.tf files). This ensures that everyone working on the project uses the same versions of providers and modules, preventing unintentional updates or discrepancies that may cause issues during deployments.

Overall, the .terraform.lock.hcl file helps maintain reproducibility and stability in Terraform projects by fixing the versions of providers and modules.
To lock the version of a Terraform module, you'll need to specify a version constraint in your Terraform code. This can be done using either the version parameter or the source parameter in your module definition.

### lock Terraform module versions with version constraint using the version parameter
  
Here's an example of how to specify a version constraint using the version parameter:

```sh
# main.tf
# Use the ec2-instance module at version 1.0.0
module "example" {
  source  = "./modules/ec2-instance"
  version = "1.0.0"

  instance_type = "t2.micro"
  ami           = "ami-0c55b159cbfafe1f0"
}
```
  
In this example, we're specifying that we want to use version 1.0.0 of the ec2-instance module. Terraform will check to see if this version of the module is available in your configured module registry and will fail if it's not.

You can also use the source parameter to specify a version constraint:

```sh
# main.tf

# Use the ec2-instance module at version 1.*.*
module "example" {
  source = "./modules/ec2-instance?ref=1.0.0"

  instance_type = "t2.micro"
  ami           = "ami-0c55b159cbfafe1f0"
}
```

In this example, we're specifying that we want to use any version of the ec2-instance module that starts with 1.. The ref parameter is used to specify the exact version to use.

By locking the version of your Terraform modules, you can ensure that your deployments are consistent and reproducible over time.
  
# Let me show you the hands-on experience of above theory about modules--->

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/f29a7489-93c8-4022-8f26-4568f077c630)

## Create two modules with complete independent configuration dependencies:

- module_1 : To provision a docker container serving website

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/4c72e57a-4f3f-49b2-a251-4eb537596736)
  
- module_2 : To provision a docker container serving website

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/ad66e6f4-227d-4549-afbf-577d58567572)
  
## Create terrafom main.tf configuration file to run both modules combinely to provision our infrastructure:
  
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/a28bb48e-0e5b-4090-8076-e6f773e7a476)

```sh
terraform init
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/9ec0e0c1-8eff-4d8a-99a2-2616a2b25876)

```sh
terraform plan
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/cdcf347c-0013-4c7c-94c7-74547945a5c0)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/fc8bc642-ac03-4ccd-8775-82c5e9337d4a)
  
```sh
terraform apply
```  
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/0fdff4a9-4d5a-4a14-9433-b3980bce632a)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/f3b0d0c5-3f4f-4e38-b20c-8ecb1d138e9c)
  
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/ea30ae34-d0dc-4793-a565-f95ef8662546)
  
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/294b2ca4-0f49-4af7-afe9-d70fe00ff58f)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/b1c1c49b-b72d-48ac-b7c3-df135954b42a)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/2c319eb9-42ad-4009-b7fc-1b81d4978ad5)

Today, We learned about modules in Terraform, their benefits, creating modules, modular composition, module versioning, and locking module versions. Modules are a powerful feature in Terraform that promote code reuse, scalability, and maintainability in infrastructure provisioning.

Keep exploring and experimenting with Terraform modules to build robust and scalable infrastructure configurations. Happy learning! 😊
