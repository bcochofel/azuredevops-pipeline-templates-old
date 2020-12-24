# Terraform Validation/Deployment

This template does `static analysis` on terraform code.

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
| preInitSteps | no | `[]` | pre terraform init steps. |
| postInitSteps | no | `[]` | post terraform init steps. |
| workingDirectory | yes | none | terraform files directory to execute commands. |
| backendKey | yes | none | terraform remote azure backend key. |

(*) See [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#pool) for more info.
