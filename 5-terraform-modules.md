# 5. Interact with Terraform modules

## 5a. Contrast module source options
```
# <NAMESPACE>/<NAME>/<PROVIDER>
# source could also be a git repository or a relative path
module "consul" {
  source = "hashicorp/consul/aws"
  version = "0.1.0"
}
```

### Private registry
```
# <HOSTNAME>/<NAMESPACE>/<NAME>/<PROVIDER>
module "vpc" {
  source = "app.terraform.io/example_corp/vpc/aws"
  version = "0.9.3"
}

```


## 5b. Interact with module inputs and outputs
### Module Inputs
You can set them directly or define `variables.tf` (and optionally also `terraform.tfvars`).
```
variable "vpc_cidr" {
  description = "CIDR block for VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "vpc_azs" {
  description = "Availability zones for VPC"
  type        = list
  default     = ["us-west-2a", "us-west-2b", "us-west-2c"]
}

variable "vpc_private_subnets" {
  description = "Private subnets for VPC"
  type        = list(string)
  default     = ["10.0.1.0/24", "10.0.2.0/24"]
}
```
### Module Outputs
```
output "vpc_public_subnets" {
  description = "IDs of the VPC's public subnets"
  value       = module.vpc.public_subnets
}

output "ec2_instance_public_ips" {
  description = "Public IP addresses of EC2 instances"
  value       = module.ec2_instances.public_ip
}
```


## 5c. Describe variable scope within modules/child modules
Child module are passed in from the parents module in the same way variables can be passed in to the module its self. They really need to be thought of as completely independent and you are just using them through the interface defined by the child module. You similarily can use a child modules output variables in the same way you can use the parent modules as defined above.

### Locals 
https://www.terraform.io/docs/configuration/locals.html  
A local value assigns a name to an expression, so you can use it multiple times within a module without repeating it.


## 5d. Discover modules from the public Terraform Module Registry
Browse in the Terraform registry: https://registry.terraform.io/. Use as in **5a**.

## 5e. Defining module version
See **5a**.