# 8. Read, generate, and modify configuration

## 8a. Demonstrate use of variables and outputs
### Variables
Variables are defined in variable blocks. Usually in `variables.tf`. They have different types, and optionally default values and descriptions. They can be passed in and defaults overridden with `terraform.tfvars` or `*.auto.tfvars` files or named something different with the -var-file flag.
```
terraform apply \
  -var-file="secret.tfvars" \
  -var-file="production.tfvars"
``` 
You can also use environment variables for string vars e.g `TF_VAR_region`.
```
# Define
variable "region" {
  default = "eu-west-2"
}

# Usage
provider "aws" {
  region = var.region
}

# Pass in
# File: terraform.tfvars
region = "eu-west-1"
```
https://www.terraform.io/docs/configuration/variables.html

### Outputs
Output values are like the return values of a Terraform module, and have several uses:
* A child module can use outputs to expose a subset of its resource attributes to a parent module.
* A root module can use outputs to print certain values in the CLI output after running terraform apply.
* When using remote state, root module outputs can be accessed by other configurations via a terraform_remote_state data source.
Resource instances managed by Terraform each export attributes whose values can be used elsewhere in configuration. Output values are a way to expose some of that information to the user of your module.
```
output "instance_ip_addr" {
  value       = aws_instance.server.private_ip
  sensitive   = true # suppress cli output
}
```
Note: Outputs are only rendered when Terraform applies your plan. Running terraform plan will not render outputs.
https://www.terraform.io/docs/configuration/outputs.html

## 8b. Describe secure secret injection best practice
Most Terraform providers require credentials to interact with a third-party service that they wrap. This provider allows such credentials to be obtained from Vault, which means that operators or systems running Terraform need only access to a suitably-privileged Vault token in order to temporarily lease the credentials for other providers.

Currently Terraform has no mechanism to redact or protect secrets that are returned via data sources, so secrets read via this provider will be persisted into the Terraform state, into any plan files, and in some cases in the console output produced while planning and applying. These artifacts must therefore all be protected accordingly.

To reduce the exposure of such secrets, the provider requests a Vault token with a relatively-short TTL (20 minutes, by default) which in turn means that where possible Vault will revoke any issued credentials after that time, but in particular it is unable to retract any static secrets such as those stored in Vault's "generic" secret backend.
https://registry.terraform.io/providers/hashicorp/vault/latest/docs  
https://learn.hashicorp.com/tutorials/terraform/secrets-vault

## 8c. Understand the use of collection and structural types
https://www.terraform.io/docs/configuration/types.html#complex-types
### Collections
* list(...): a sequence of values identified by consecutive whole numbers starting with zero.
* map(...): a collection of values where each is identified by a string label.
* set(...): a collection of unique values that do not have any secondary identifiers or ordering
### Structures
* object(...): a collection of named attributes that each have their own type.
```
{
  name = "John"
  age  = 52
}
```
* tuple(...): a sequence of elements identified by consecutive whole numbers starting with zero, where each element has its own type.
```
["a", 15, true]
```


## 8d. Create and differentiate resource and data configuration
### Resources
Resources are the resource blocks.  
https://www.terraform.io/docs/configuration/resources.html

### Data
Data sources allow data to be fetched or computed for use elsewhere in Terraform configuration. Use of data sources allows a Terraform configuration to make use of information defined outside of Terraform, or defined by another separate Terraform configuration.

Each provider may offer data sources alongside its set of resource types.
```
# Find the latest available AMI that is tagged with Component = web
data "aws_ami" "web" {
  filter {
    name   = "state"
    values = ["available"]
  }

  filter {
    name   = "tag:Component"
    values = ["web"]
  }

  most_recent = true
}
```
https://www.terraform.io/docs/configuration/data-sources.html


## 8e. Use resource addressing and resource parameters to connect resources together
* Resources: `resource_type.resource_name[resource index]`.
* Modules: `module.module_name[module index]`.

https://www.terraform.io/docs/internals/resource-addressing.html


## 8f. Use Terraform built-in functions to write configuration
https://www.terraform.io/docs/configuration/functions.html
Lots of common functionality is available through built-in functions. Syntax for usage and a few examples can be seen below:
```
max(5, 12, 9)

> join(", ", ["foo", "bar", "baz"])
foo, bar, baz
> join(", ", ["foo"])
foo

# file reads the contents of a file at the given path and returns them as a string.
file(path)

> timestamp()
2018-05-13T07:44:12Z

> base64sha256("hello world")
uU0nuZNNPgilLlLX2n2r+sSE7+N6U4DukIj3rOLvzek=
```


## 8g. Configure resource using a dynamic block
### Dynamic nested blocks
* https://www.terraform.io/docs/configuration/expressions.html#dynamic-blocks
* https://www.hashicorp.com/blog/hashicorp-terraform-0-12-preview-for-and-for-each
```
resource "aws_elastic_beanstalk_environment" "tfenvtest" {
  name                = "tf-test-name"
  application         = "${aws_elastic_beanstalk_application.tftest.name}"
  solution_stack_name = "64bit Amazon Linux 2018.03 v2.11.4 running Go 1.12.6"

  dynamic "setting" {
    for_each = var.settings
    content {
      namespace = setting.value["namespace"]
      name = setting.value["name"]
      value = setting.value["value"]
    }
  }
}
```
Only use when required because it makes configuration more confusing. So, only when you want to use data of a unknown length.

### Resource loops
https://www.terraform.io/docs/configuration/resources.html#for_each-multiple-resource-instances-defined-by-a-map-or-set-of-strings
Use `count` if resources are almost identical. Use `for_each` if you need to template in different values using a map or set type var.
```
# Using Map
resource "azurerm_resource_group" "rg" {
  for_each = {
    a_group = "eastus"
    another_group = "westus2"
  }
  name     = each.key
  location = each.value
}
```

### Conditional Resouces
`count = var.target_group_addition ? 1 : 0`


## 8h. Describe built-in dependency management (order of execution based)
Resources are created in parallel. But dependencies can be made explicit so it will order execution and wait to obey the configuration as required. Very common and very useful. `depends_on = [aws_s3_bucket.example, aws_instance.example_c]`.
