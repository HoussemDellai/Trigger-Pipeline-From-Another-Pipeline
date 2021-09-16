# Trigger-Pipeline-From-Another-Pipeline
Triggering a pipeline from another one in Azure DevOps.

There are 2 solutions for triggering a pipeline from another pipeline in Azure DevOps Pipelines using yaml syntax:
1) Using 'resources' feature 
2) Using yaml templates

Let's explore both options.

1) Trigger a pipeline from another pipeline using 'resources' feature

Here is below an example of how that works. The second pipeline will be triggered after the first one finishes successfully.
```YAML
# azure-pipelines.yml
name: FirstPipeline

trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: echo This pipeline runs first and will trigger a second pipeline !
```

```YAML
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
  - script: echo This pipeline was triggered by another pipeline.
```
