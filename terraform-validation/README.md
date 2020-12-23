# Terraform Validation

This template does `static analysis` on terraform code.

# Parameters

The template accepts the following parameters.

| parameter | required | default | description |
| --------- | -------- | ------- | ----------- |
| debug | no | `false` | whether or not to enable debug for the pipeline. |
| varGroup | no | `{}` | variable groups to include on the pipeline. |
| vmImage | no | `ubuntu-latest` | pool vmImage to use (*) |
| installTerraformVer | no | `0.13.4` | if not empty installs this terraform version. |
| installConftestVer | no | `null` | if not empty installs this conftest version and runs conftest validation. |
| installTFLintVer | no | `null` | if not empty installs this tflint version and runs tflint validation. |
| tflintAzurermRulesetVer | no | `0.5.0` | TFLint Azurerm Ruleset Version (applies only if tflint is going to be installed) |

(*) See [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#pool) for more info.
