# 9. Understand Terraform Cloud and Enterprise capabilities
Terraform Enterprise is our self-hosted distribution of Terraform Cloud. It offers enterprises a private instance of the Terraform Cloud application, with no resource limits and with additional enterprise-grade architectural features like audit logging and SAML single sign-on.

https://www.terraform.io/docs/enterprise/index.html

## 9a. Describe the benefits of Sentinel, registry, and workspaces
### Sentinel
Sentinel is an embedded policy-as-code framework integrated with the HashiCorp Enterprise products. It enables fine-grained, logic-based policy decisions, and can be extended to use information from external sources.

These can be used to define andf restrict how terraform can be used safely in your ogranisation in an automated way.

### Registry
Terraform Cloud's private module registry helps you share Terraform modules across your organization. It includes support for module versioning, a searchable and filterable list of available modules, and a configuration designer to help you build new workspaces faster.

### Workspaces
#### Planning and Organizing Workspaces
We recommend that organizations break down large monolithic Terraform configurations into smaller ones, then assign each one to its own workspace and delegate permissions and responsibilities for them. Terraform Cloud can manage monolithic configurations just fine, but managing smaller infrastructure components like this is the best way to take full advantage of Terraform Cloud's governance and delegation features.

For example, the code that manages your production environment's infrastructure could be split into a networking configuration, the main application's configuration, and a monitoring configuration. After splitting the code, you would create "networking-prod", "app1-prod", "monitoring-prod" workspaces, and assign separate teams to manage them.

Much like splitting monolithic applications into smaller microservices, this enables teams to make changes in parallel. In addition, it makes it easier to re-use configurations to manage other environments of infrastructure ("app1-dev," etc.).


## 9b. Differentiate OSS and Terraform Cloud workspaces
Terraform Cloud and Terraform CLI both have features called "workspaces," but they're slightly different. CLI workspaces are alternate state files in the same working directory; they're a convenience feature for using one configuration to manage multiple similar groups of resources.

Terraform Cloud workspaces and local working directories serve the same purpose, but they store their data differently:

### Workspace Contents
Terraform Cloud workspaces and local working directories serve the same purpose, but they store their data differently:

Component | Local Terraform | Terraform Cloud
--|--|--
Terraform configuration | On disk | In linked version control repository, or periodically uploaded via API/CLI
Variable values | As `.tfvars` files, as CLI arguments, or in shell environment | In workspace
State | On disk or in remote backend | In workspace
Credentials and secrets | In shell environment or entered at prompts | In workspace, stored as sensitive variables

In addition to the basic Terraform content, Terraform Cloud keeps some additional data for each workspace:

- **State versions:** Each workspace retains backups of its previous state files. Although only the current state is necessary for managing resources, the state history can be useful for tracking changes over time or recovering from problems. (See also: [State](./state.html).)
- **Run history:** When Terraform Cloud manages a workspace's Terraform runs, it retains a record of all run activity, including summaries, logs, a reference to the changes that caused the run, and user comments. (See also: [Viewing and Managing Runs](../run/manage.html).)


## 9c. Summarize features of Terraform Cloud
### Running
https://www.terraform.io/docs/cloud/run/ui.html
Terraform Cloud has three workflows for managing Terraform runs.
* The UI/VCS-driven run workflow described below, which is the primary mode of operation.
* The API-driven run workflow, which is more flexible but requires you to create some tooling.
* The CLI-driven run workflow, which uses Terraform's standard CLI tools to execute runs in Terraform Cloud.

In the UI and VCS workflow, every workspace is associated with a specific branch of a VCS repo of Terraform configurations. Terraform Cloud registers webhooks with your VCS provider when you create a workspace, then automatically queues a Terraform run whenever new commits are merged to that branch of workspace's linked repository.

Terraform Cloud also performs a speculative plan when a pull request is opened against that branch. Terraform Cloud posts a link to the plan in the pull request, and re-runs the plan if the pull request is updated.

The Terraform code for a normal run always comes from version control, and is always associated with a specific commit.

### Features
https://www.hashicorp.com/products/terraform/pricing
* IaC
* Colaborative IaC
* Manage & enforce teams & policies (as code)
* Advanced Security, Compliance, and Governance - SSO, Audit, Private Datacenter Networking
* Self-Service Infrastructure - Support for ServiceNow / Configuration Designer workflows
* Performance - Additional concurrent runs
* Support 
