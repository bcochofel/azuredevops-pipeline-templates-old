# Terragrunt Validation/Deployment

This template does `static analysis` on terraform code and deploys the code using terragrunt.

# Parameters

The template accepts the following parameters.

| parameter | required | default | description |
| --------- | -------- | ------- | ----------- |
| debug | no | `false` | whether or not to enable debug for the pipeline. |
| varGroup | no | `{}` | variable groups to include on the pipeline. |
| vmImage | no | `ubuntu-latest` | pool vmImage to use (*) |
| installTerraform | no | `true` | whether or not to install Terraform. |
| terraformVersion | no | `0.13.4` | terraform version to install (if `installTerraform` is true) |
| installConftest | no | `true` | whether or not to install Conftest. |
| conftestVersion | no | `0.21.0` | conftest version to install (if `installConftest` is true) |
| installTFLint | no | `true` | whether or not to install TFLint. |
| tflintVersion | no | `0.20.2` | tflint version to install (if `installTFLint` is true) |
| tflintRulesetAzurermVersion | no | `0.5.0` | AzureRM TFLint Ruleset version. |
| installTerragrunt | no | `true` | whether or not to install Terragrunt. |
| terragruntVersion | no | `0.26.7` | terragrunt version to install (if `installTerragrunt` is true) |
| terragruntRunAll | no | `false` | whether or not the execute `validate-all plan-all apply-all` commands. |
| workingDirectory | yes | none | terraform files directory to execute commands. |
| environmentVars | no | `{}` | environment variables to be added. |
| prComments | no | `true` | whether or not tp do Azure DevOps PR comments for terraform plan. |
| preInitSteps | no | `[]` | pre terraform init steps. |
| postInitSteps | no | `[]` | post terraform init steps. |
| planExtraArgs | no | `null` | extra arguments for terraform plan command. |
| prePlanSteps | no | `[]` | pre terraform plan steps. |
| postPlanSteps | no | `[]` | post terraform plan steps. |
| environment | yes | `null` | environment to deploy infrastructure. (**) |
| validationOnly | no | `false` | set to true if you only want to execute the validation stage. |
| applyExtraArgs | no | `null` | extra arguments for terraform apply command. |
| preApplySteps | no | `[]` | pre terraform apply steps. |
| postApplySteps | no | `[]` | post terraform apply steps. |

(*) See [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#pool) for more info.
(**) If you want to approve before deploy use one of the [Azure DevOps environments](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/environments?view=azure-devops) and apply the rules you want.

# Examples

## Pipeline for Terraform Static Analysis

```yaml
name: $(BuildDefinitionName)_$(date:yyyyMMdd)$(rev:.r)

trigger: none
pr:
  branches:
    include:
      - '*'
  paths:
    include:
      - examples/network
    exclude:
      - examples/network/README.md

resources:
  repositories:
    - repository: templates
      type: github
      name: bcochofel/azuredevops-pipeline-templates
      ref: refs/heads/main
      endpoint: GitHubConnection

stages:
  - template: terraform/terragrunt.yml@templates
    parameters:
      environment: sandbox
      validationOnly: true
      varGroups:
        - terraform-configuration
        - terraform-configuration-secrets
      workingDirectory: examples/network
      prComments: false
      terragruntRunAll: false
```

**NOTES:**
* The `prComments` parameter needs to be set to `false` if not using Azure DevOps repositories.
* The `terragruntRunAll` can be set to `true` to deploy all the modules in a region or environment.

## Pipeline for Terraform Deployment

```yaml
name: $(BuildDefinitionName)_$(date:yyyyMMdd)$(rev:.r)

trigger:
  branches:
    include:
      - master
      - main
  paths:
    include:
      - examples/network
    exclude:
      - examples/network/README.md
pr: none

resources:
  repositories:
    - repository: templates
      type: github
      name: bcochofel/azuredevops-pipeline-templates
      ref: refs/heads/main
      endpoint: GitHubConnection

stages:
  - template: terraform/terragrunt.yml@templates
    parameters:
      environment: sandbox
      varGroups:
        - terraform-configuration
        - terraform-configuration-secrets
      workingDirectory: examples/network
      prComments: false
      terragruntRunAll: false
```

**NOTES:**
* The `prComments` parameter needs to be set to `false` if not using Azure DevOps repositories.
* The `terragruntRunAll` can be set to `true` to deploy all the modules in a region or environment.
