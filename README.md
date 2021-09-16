# Trigger Pipeline from another Pipeline in Azure DevOps

There are 2 solutions for triggering a pipeline from another pipeline in Azure DevOps Pipelines using yaml syntax:
1) Using 'resources' feature 
2) Using yaml templates

Let's explore both options.

## 1) Trigger a pipeline from another pipeline using 'resources' feature

Here is below an example of how that works. The second pipeline will be triggered after the first one finishes successfully.
```YAML
# azure-pipelines.yml
name: FirstPipeline

trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: echo This pipeline runs first !
```

```YAML
# azure-pipelines-trigger.yml
name: SecondPipeline 

trigger: none

# this pipeline will be triggered by another pipeline
resources:
  pipelines:
  - pipeline: previous-pipeline-trigger   # Name of the pipeline resource
    source: First-Pipeline-yaml # Name of the pipeline referenced by the pipeline resource
    project: Trigger-Pipeline-From-Another-Pipeline # Required only if the source pipeline is in another project
    trigger: true # enable the trigger

pool:
  vmImage: ubuntu-latest

steps:
  - script: echo This pipeline was set to be triggered after first pipeline completes.
```

Note how we set the trigger for the second pipeline: 'trigger: none'. This is to trigger the pipeline when only after the first one completes (i.e not after commit or PR).

More details about resources: https://docs.microsoft.com/en-us/azure/devops/pipelines/process/resources

## 2) Trigger a pipeline from another pipeline using YAML Templates

```YAML
# azure-pipelines.yml
name: FirstPipeline

trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: echo This pipeline runs first and will trigger a second pipeline !
- template: 2_azure-pipelines-template.yml
```

```YAML
# azure-pipelines-template.yml
steps:
- script: echo This pipeline will be triggered by another pipeline !
```

More details about templates: https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates
