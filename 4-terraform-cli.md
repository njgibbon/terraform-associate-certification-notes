# 4a. Given a scenario: choose when to use terraform fmt to format code
https://www.terraform.io/docs/commands/fmt.html
```
terraform fmt [options] [DIR]
# options
-check - check don't change
-recursive - by default only current dir
-diff - output diff of formatting changes
```


# 4b. Given a scenario: choose when to use terraform taint to taint Terraform resources
https://www.terraform.io/docs/commands/taint.html  
A taint marks a resource as 'tainted' so it will be destroyed and recreated on the next apply.
```
terraform taint [options] address
# where address is resource address syntax e.g aws_instance.foo
terraform taint "module.route_tables.azurerm_route_table.rt[\"DefaultSubnet\"]"
```


# 4c. Given a scenario: choose when to use terraform import to import existing infrastructure into your Terraform state
https://www.terraform.io/docs/commands/import.html
```
terraform import [options] ADDRESS ID
```


# 4d. Given a scenario: choose when to use terraform workspace to create workspaces
https://www.terraform.io/docs/state/workspaces.html  
Backends can have multiple instances to store state called workspaces.
```
terraform workspace new bar
terraform workspace select default
# default always exists and can not be deleted
```


# 4e. Given a scenario: choose when to use terraform state to view Terraform state
https://www.terraform.io/docs/commands/state/index.html
All `terraform state` subcommands write a backup file. The location of this can be configured with `-backup`.
```
terraform state list
# filter by resource, moudle or id
terraform state show <resource-address> # to show single resource
# also 
mv # move
pull # pull and output to stdout
push # push local state to remote
replace-provider # replaces provider
rm # remove resources from state
```

# 4f. Given a scenario: choose when to enable verbose logging and what the outcome/value is
https://www.terraform.io/docs/internals/debugging.html
```
export TF_LOG=TRACE|DEBUG|INFO|WARN|ERROR
export TF_LOG_PATH=specific-file.txt
# Crash log automatically saved to crash.log and most interesting part is 'panic'
```