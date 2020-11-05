# 3. Understand Terraform basics

## 3a. Handle Terraform and provider installation and versioning
### Install
Lots of ways to install. Depending on Platform. Terraform is a single binary that needs to be accessible in the system and is named `terraform`. You can always choose to download specific versions.

### Verify
```
terraform -help
terraform version
```

### Terraform Settings
* These go in a `terraform` block.
* You can pin your terraform to a specific version in using `required_version`.
* `required_providers` block can be used to pin acceptable provider versions.
* Backends define where and how operations are performed. Here is where remote state configurations are defined. A backend block can not refer to named values likes input variables or data sources.

### Providers
* A Provider is a plugin which translates the DSL into API calls for the service. The 'AWS' provider does this for AWS. Configured with a provider block.
* They are given a local name. Namespace conflict can be handled by using a compound local name if needed. https://www.terraform.io/docs/configuration/provider-requirements.html#handling-local-name-conflicts
* A providers documentation should define required arguments.
* Aliases can be used so that you can have multiple configurations for the same provider. Usage: `aws.provider_name.*`.
* Since 0.13 none-hashicorp distributed providers are a thing.


## 3b. Describe plug-in based architecture
* A plug-in based architecture enables 100s of service providers to start working with terraform with distributed development.
* `terraform init` will download the providers defined in the configuration and then subsequent commands will use local settings and data.


## 3c. Demonstrate using multiple providers
Define and use as expected with multiple entries in `required_providers` and multiple `provider` blocks. You can use aliases to use different instances of the same provider with different provider configuration.


## 3d. Describe how Terraform finds and fetches providers
```
terraform {
  required_providers {
    mycloud = {
      source  = "hashicorp/aws"
      version = "~> 1.0.4"
    }
  }
}
```
### Source
Source addresses consist of three parts delimited by slashes (/), as follows:
```
<HOSTNAME>/<NAMESPACE>/<TYPE>
# hostname not needed if it's terraform
registry.terraform.io/hashicorp/random
```
### Versions
```
~> shorthand for only patch releases
>= greater than or equal to
```

## 3e. Explain when to use and not use provisioners and when to use local-exec or remote-exec
### Provisioners
Provisioners can be used to model specific actions on the local machine or on a remote machine in order to prepare servers or other infrastructure objects for service.

Provisioners should be a last resort and are only a feature for the sake of pragmatism.

Reccomend to use packer instead of post-install config with terraform.
### local-exec and remote-exec
```
resource "aws_instance" "web" {
  # ...

  provisioner "remote-exec" {
    inline = [
      "puppet apply",
      "consul join ${aws_instance.web.private_ip}",
    ]
  }
}
```
Remote runs on remote machine. Local runs where ever terraform is running from and so you must ensure that either environment has the additional dependencies available.