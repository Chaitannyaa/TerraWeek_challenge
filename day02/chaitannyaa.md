# TerraWeek Day 2/7

# Task 1: Familiarize yourself with HCL syntax used in Terraform

## Learn about HCL blocks, parameters, and arguments

HCL (HashiCorp Configuration Language) is the language used in Terraform for defining infrastructure resources in code. When writing Terraform code in HCL, you typically define resources using a block declaration, specify parameters within the block, and then provide arguments to those parameters.

Block is a container that groups together a set of parameters to define a specific object or resource. Blocks are a fundamental aspect of HCL and are commonly used when working with infrastructure provisioning tool such as Terraform.

The block syntax in HCL is defined by curly braces `{}` and consists of a block type and one or more attributes or parameters. The block type defines the kind of object or resource being created, while the attributes specify the properties or attributes of the object being created.

```hcl
block_Object "block_type" "label" {
    parameter_1 = argument_1
    parameter_2 = argument_2
    parameter_3 = argument_3
}
```

For example, below is a simple HCL block that creates an AWS S3 bucket resource:

```hcl
resource "aws_s3_bucket" "example" {
bucket = "my-example-bucket"
acl = "private"
}
```

In this example, the block type is `aws_s3_bucket`, which defines an AWS S3 bucket resource. The block contains two parameters, `bucket` and `acl`, which are defined using the `=` delimiter. The `aws_s3_bucket.example` syntax is used to reference the bucket resource instance elsewhere in the Terraform configuration.

Here is an example block, parameter, and argument using the `aws_instance` resource:

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "example-instance"
  }
}
```

In this example, `aws_instance` is the block type, `example` is the name of the resource instance, `ami` and `instance_type` are parameters specific to the `aws_instance` resource, and `ami-0c55b159cbfafe1f0` and `t2.micro` are the arguments for those parameters.

You can think of a block as a container for parameters that define a specific resource type, such as `aws_instance`, `aws_subnet`, or `aws_security_group`. Each block also has a unique identifier that is used to reference the resource throughout your Terraform code. In the example above, the identifier is "example" and the full reference to the resource is `aws_instance.example`.

Parameters are properties specific to the block type, such as `ami` and `instance_type` for the `aws_instance` block. These are used to define the resource's behavior, and can accept one or more arguments.

Lastly, arguments are the values assigned to the parameters, such as `ami-0c55b159cbfafe1f0` or `t2.micro` in the example above. These are the values that will be set for the resource when it is created or updated in your cloud provider.

## Explore the different types of resources and data sources available in Terraform

Sure, here are the different types of resources and data sources available in Terraform:

1. Resources: Resources are the fundamental building blocks of Terraform configurations. They represent the infrastructure objects that need to be created, updated, or deleted. Some of the commonly used resources in Terraform are:

**aws_instance:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.example.id

  # Other configuration options like security groups, tags, etc.
}
```

**aws_security_group:
```hcl
resource "aws_security_group" "example" {
  name        = "example-security-group"
  description = "Example security group"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Other ingress and egress rules
}
```

**aws_s3_bucket:
```hcl
resource "aws_s3_bucket" "example" {
  bucket = "example-bucket"
  acl    = "private"

  # Other bucket configuration options
}
```

**aws_rds_instance:
```hcl
resource "aws_rds_instance" "example" {
  engine            = "mysql"
  instance_class    = "db.t2.micro"
  allocated_storage = 10

  # Other database configuration options
}
```

**aws_lambda_function:
```hcl
resource "aws_lambda_function" "example" {
  function_name = "example-function"
  runtime       = "python3.8"
  handler       = "lambda_function.lambda_handler"

  # Other function configuration options like code, environment variables, etc.
}
```
**google_compute_instance:
```hcl
resource "google_compute_instance" "example" {
  name         = "example-instance"
  machine_type = "n1-standard-1"
  zone         = "us-central1-a"

  # Other instance configuration options like image, network, etc.
}
```

**google_storage_bucket:
```hcl
resource "google_storage_bucket" "example" {
  name          = "example-bucket"
  location      = "us-central1"

  # Other bucket configuration options
}
```

**azurerm_virtual_machine:
```hcl
resource "azurerm_virtual_machine" "example" {
  name                  = "example-vm"
  location              = "eastus"
  resource_group_name   = azurerm_resource_group.example.name
  vm_size               = "Standard_DS1_v2"

  # Other virtual machine configuration options like image, networking, etc.
}
```

# Task 2: Understand variables, data types, and expressions in HCL

### Variable.tf syntax:
```hcl
variable "variable_name" {
  description = "Write something about variable use or related to it"
  default     = "variable_value"
}
```

1. Defining Variables in HCL, few examples:

### Variable.tf
```hcl
variable "region" {
  description = "The AWS region to deploy resources"
  default     = "us-west-2"
}

variable "instance_type" {
  description = "The type of EC2 instance"
  type        = string
  default     = "t2.micro"
}

variable "enable_backup" {
  description = "Enable backup for the resources"
  type        = bool
  default     = true
}
```

2. Using Variables in HCL Expressions:

### main.tf:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = var.instance_type
  subnet_id     = aws_subnet.example.id

  tags = {
    Name        = "Example Instance"
    Environment = "Development"
  }
}
```

3. Performing Expressions and Functions in HCL:

### main.tf:
```hcl
resource "aws_ebs_volume" "example" {
  availability_zone = "us-west-2a"
  size              = 100
  encrypted         = true

  # Constructing the volume name using an expression
  tags = {
    Name = "Volume ${var.region}-${aws_instance.example.id}"
  }

  # Using a function to generate a random ID for the volume
  snapshot_id = data.aws_ebs_snapshot.example.id
  volume_id   = random_id.volume_id.hex
}
```

4. Working with Data Types in HCL:

In HCL (HashiCorp Configuration Language) used in Terraform, the following data types are commonly used:

**string**: Represents a sequence of characters. Strings are enclosed in double quotes (").

```hcl
variable "name" {
  type    = string
  default = "John Doe"
}

resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
  tags = {
    Name = var.name
  }
}
```

**number**: Represents numerical values, including both integers and floating-point numbers.

```hcl
variable "count" {
  type    = number
  default = 5
}

resource "aws_instance" "example" {
  count         = var.count
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
}

**bool**: Represents boolean values, which can be either true or false.

variable "enable_logging" {
  type    = bool
  default = true
}

resource "aws_s3_bucket" "example" {
  bucket = "my-bucket"
  acl    = var.enable_logging ? "private" : "public-read"
}
```

**list**: Represents an ordered collection of values of the same or different data types. Lists are defined using square brackets ([]) and can contain any valid Terraform data type.

```hcl
variable "ports" {
  type    = list(number)
  default = [80, 443, 8080]
}

resource "aws_security_group" "example" {
  name        = "example-security-group"
  description = "Example security group"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  dynamic "ingress" {
    for_each = var.ports

    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

**map**: Represents an unordered collection of key-value pairs. Maps are defined using curly braces ({}) and can contain any valid Terraform data types.

```hcl
variable "tags" {
  type    = map(string)
  default = {
    Environment = "Development"
    Owner       = "John Doe"
  }
}

resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
  tags          = var.tags
}
```

**object**: Represents a complex data structure that combines multiple attributes and values into a single entity. Objects are similar to maps but have a defined structure with specific attribute names and data types.

```hcl
variable "server" {
  type = object({
    name     = string
    cpu      = number
    memory   = number
    disk     = number
    location = string
  })

  default = {
    name     = "web-server"
    cpu      = 2
    memory   = 8
    disk     = 100
    location = "us-west-2"
  }
}

resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
  cpu_core      = var.server.cpu
  # Other configuration options
}
```

**tuple**: Represents an ordered collection of values of different data types. Tuples are defined using parentheses (()) and can contain any valid Terraform data types.

```hcl
variable "subnet_ids" {
  type    = tuple(string)
  default = ("subnet-abc123", "subnet-def456", "subnet-ghi789")
}

resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
  subnet_id     = element(var.subnet_ids, count.index)
  count         = length(var.subnet_ids)
}
```

**set**: Represents an unordered collection of unique values. Sets are defined using curly braces ({}) and can contain any valid Terraform data types.

```hcl
variable "allowed_regions" {
  type    = set(string)
  default = ["us-east-1", "us-west-2"]
}

resource "aws_security_group" "example" {
  name        = "example-security-group"
  description = "Example security group"

  # Allow inbound traffic from the allowed regions
  dynamic "ingress" {
    for_each = var.allowed_regions

    content {
      from_port   = 0
      to_port     = 65535
      protocol    = "-1"
      cidr_blocks = [cidrsubnet("10.0.0.0/16", 8, 0)] # Example CIDR block
    }
  }
}
```

**any**: Represents a dynamic data type that can hold any value, allowing flexibility in handling different data types.

```hcl
variable "dynamic_value" {
  type = any
  default = {
    name  = "John Doe"
    age   = 30
    email = "johndoe@example.com"
  }
}

resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
  tags = var.dynamic_value
}
```

These data types provide the foundation for defining variables, expressions, and complex data structures in Terraform configurations using HCL. Understanding and utilizing these data types effectively can greatly enhance the flexibility and versatility of your infrastructure provisioning code.

### main.tf:
```hcl
variable "ports" {
  description = "A list of ports to open"
  type        = list(number)
  default     = [80, 443, 8080]
}

resource "aws_security_group" "example" {
  name        = "example-security-group"
  description = "Example security group"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Using a for-each loop to dynamically allow ports
  dynamic "ingress" {
    for_each = var.ports

    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

In the examples above, we defined variables with different data types, used them in resource blocks, performed expressions and functions, and worked with lists. These snippets highlight the flexibility and power of HCL when it comes to defining infrastructure configurations with dynamic values and expressions.

## Create a variables.tf file and define a variable

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/7a42b4ea-3cfe-4567-956c-10e3f535e8c2)

## Use the variable in a main.tf file to create a "local_file" resource

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/7a5a4e9f-f003-4297-b68f-71633488e110)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/51aec1e4-a146-433f-9158-c8f268d3e6d1)

```sh
terraform init
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/e5ddca34-39bd-453e-b466-a142a9562f4d)

```sh
terraform plan
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/29aba4c5-4e81-4e42-a17a-2e34017c0c70)

```sh
terraform apply
```
![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/13c84267-5557-4cb3-b68c-89bde433dcc3)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/49d10c4a-9c57-4cae-bb2e-e8cca23b10d5)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/de1bd562-a6eb-4500-ba0b-a01beb02a6d4)

# Task 3: Practice writing Terraform configurations using HCL syntax

## Add required_providers to your configuration, such as Docker or AWS

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/41447fa0-e776-41fb-8126-5c3079b4047e)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/5ce5757f-2c75-4117-91c5-f700bce12832)

## Test your configuration using the Terraform CLI and make any necessary adjustments

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/7981674a-8ae8-456f-a197-3802f897f70b)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/296d0e08-dca2-464b-945a-02d51d815040)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/d76fa415-19e4-483b-91c5-54fd4dbab1f7)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/2444cccc-d023-46ba-945f-acdf57d583ab)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/c843a5c1-e44f-4eb2-9c6f-3a4440ffe81e)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/fc5badb9-9b37-4956-a828-62fbbf4ab0a2)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/577eb39c-7937-4e8d-aad9-eacde15c26da)

![image](https://github.com/Chaitannyaa/TerraWeek_challenge/assets/117350787/5d074f41-147f-4d3a-8f28-eb688ff85127)

Watch this 👉 https://youtu.be/kqJIKjkJ1Lo

Happy Learning :)
