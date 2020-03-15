# Introduction

Azure DevOps multi-stage deployment pipeline for Terraform configuration.

## How to use it

If you have Terraform configuration in your project ready to be deployed, you need to create `azure-pipelines.yml` file in your repository root directory and add a similar configuration as below.

```yaml
pool: your-agent-pool-name

trigger:
  batch: true
  branches:
    exclude:
      - refs/heads/feature*

resources:
  repositories:
    - repository: azure-devops-terraform-pipeline-template
      type: github
      name: viktors-telle/azure-devops-terraform-pipeline-template
      ref: "refs/heads/master"

stages:
  - template: main.yml@viktors-telle/azure-devops-terraform-pipeline-template
    parameters:
      container_name: blob_container_name
      container_key: blob_file_name
      variable_group_name: project_variable_group_name
```

`container_name` - the blob container name where project-related Terraform state files are stored.

`container_key` - the blob name of Terraform state file. For example, `storage-acccount`, `redis-cache`, etc.

`variable_group_name` - [optional] Azure DevOps variable group where project-related variables are stored and should be transformed during deployment.
If you want to replace variable values during pipeline execution then you need to create a new variable group in Azure DevOps for each environment separately and use environment postfix, e.g. `resource-group-dev`, `resource-group-stage`, `resource-group-prod` and pass the name without postfix.
The rest of the parameters will come from the Azure DevOps variable group `terraform-[environment]`.
