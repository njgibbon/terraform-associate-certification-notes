# 6. Navigate Terraform workflow

## 6a. Describe Terraform workflow ( Write -> Plan -> Create )
https://www.terraform.io/guides/core-workflow.html
```
# create .tf files
terraform plan
terraform apply
# yes or -auto-approve
```


## 6b. Initialize a Terraform working directory (terraform init)
https://www.terraform.io/docs/commands/init.html  
```
terraform init
```


## 6c. Validate a Terraform configuration (terraform validate)
https://www.terraform.io/docs/commands/validate.html
```
# requires init but not necessarily remote backend
terraform init -backend=false
terraform validate
```
Validate runs checks that verify whether a configuration is syntactically valid and internally consistent, regardless of any provided variables or existing state. It is thus primarily useful for general verification of reusable modules, including correctness of attribute names and value types.


## 6d. Generate and review an execution plan for Terraform (terraform plan)
https://www.terraform.io/docs/commands/plan.html
```
terraform plan
```
Creates execution plan. Tells you what will be created and destroyed and what will stay the same. 
The optional -out argument can be used to save the generated plan to a file for later execution with terraform apply, which can be useful when running Terraform in automation.


## 6e. Execute changes to infrastructure with Terraform (terraform apply)
https://www.terraform.io/docs/commands/apply.html
```
terraform apply [options] [dir-or-plan]
# yes or -auto-approve
```
Performs a plan by default unless a plan is passed in as an argument. Apply executes the changes to create the desired state.


## 6f. Destroy Terraform managed infrastructure (terraform destroy)
https://www.terraform.io/docs/commands/destroy.html
```
terraform destory
```
Destroys all of the infrastructure which would otherwise be planned for creation.
This command accepts all the arguments and flags that the apply command accepts, with the exception of a plan file argument.
