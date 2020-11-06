# 7. Implement and maintain state

## 7a. Describe default local backend
https://www.terraform.io/docs/backends/index.html
A backend is primarily for how state is loaded. Local is default. `terraform.tfstate` in current dir.


## 7b. Outline state locking
https://www.terraform.io/docs/state/locking.html  
Mutex lock to ensure no changes happen to state at the same time. Local backend uses system APIs. Other backends use some specified data store.


# 7c. Handle backend authentication methods
Authentication will be defined in the backend config. Or assumed by a backend otherwise. i.e aws will take your default profile. This will be documented with the backend. The entity running terraform needs permission to adjust state locks and to read and write to the state storage mechanism.


# 7d. Describe remote state storage mechanisms and supported standard backends
```
terraform {
  backend "s3" {
    bucket = "mybucket"
    key    = "path/to/my/key"
    region = "us-east-1"
  }
}
# can be done on cli instead of defined backend block
terraform init \
    -backend-config="address=demo.consul.io" \
    -backend-config="path=example_app/terraform_state" \
    -backend-config="scheme=https"
```
### Standard
Handles state storage and state locking. Standard backends e.g S3, Consul, artifactory.
### Enchanced
All features of standard plus remote execution. Only currently avaiailable with Terraform Cloud.


# 7e. Describe effect of Terraform refresh on state
https://www.terraform.io/docs/commands/refresh.html
```
terraform refresh
```
The terraform refresh command is used to reconcile the state Terraform knows about (via its state file) with the real-world infrastructure. This can be used to detect any drift from the last-known state, and to update the state file.


# 7f. Describe backend block in configuration and best practices for partial configurations
You do not need to specify every required argument in the backend configuration. Omitting certain arguments may be desirable to avoid storing secrets, such as access keys, within the main configuration. When some or all of the arguments are omitted, we call this a partial configuration.  
You can supply the arguments interactively, via a file (see **7d**) or CLI KVPs.  
When supplying a partial configuration the backend name block must at least be supplied so terraform know which backend is beign used:
```
terraform {
  backend "consul" {}
}
```


# 7g. Understand secret management in state files
https://www.terraform.io/docs/state/sensitive-data.html
Use backends which encrypt data at rest and in transit and have additional features such as user IAM and audit logging.

