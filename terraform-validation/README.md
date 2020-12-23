# Terraform Validation

This template does `static analysis` on terraform code.

# Parameters

The template accepts the following parameters.

| parameter | required | default | description |
| --------- | -------- | ------- | ----------- |
| debug | no | `false` | whether or not to enable debug for the pipeline. |
| varGroup | no | `{}` | variable groups to include on the pipeline. |
| vmImage | no | `ubuntu-latest` | pool vmImage to use (*) |

(*) See [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#pool) for more info.
