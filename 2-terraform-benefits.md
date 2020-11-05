# Understand Terraform's purpose (vs other IaC)
## 2a. Explain multi-cloud and provider-agnostic benefits
### Multi-cloud benefits
* Fault tolerance across providers.
* No vendor lock.
* Ability to use the best product or prices regardless of data center location.

### Terraform helps
* Terraform means control from one place. No duplication of effort. Including cross-cloud dependencies.


## 2b. Explain the benefits of state
* Enables the dependency graph and execution plan.
* Tracks the real world.
* Enables idempotency. Only things that require changes will change.
* Lets you verify that the changes that will occur are those expected.
* Performance. Slow to look up all resources.
* Metadata - Key functional notes on resources such as dependencies and how to destroy.