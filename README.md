# flyway-azure
flyway docker image for the azure pipeline

## Getting started

The easiest way to get started is simply to test the image by running

`docker run --rm flyway [flyway cli arguments here]`

## Azure Pipleline

The Azure Pipelines system requires a few things in 

### Linux-based containers:

* Bash
* glibc-based
* Can run Node.js (which the agent provides)
* Does not define an ENTRYPOINT
* USER has access to groupadd and other privileges commands without sudo

### agent host:

* Ensure Docker is installed
* The agent must have permission to access the Docker daemon

### Requirement for the non glibc-based containers

* Add a Node LTS binary to your container.
* Set container label "com.azure.dev.pipelines.handler.node.path" to the Node.js binary
* Add requirements

`RUN apk add bash sudo shadow`

Refer to [Define container jobs (YAML)](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/container-phases) for more information.

### Azure Pipeline Example

```
stages:
- stage: DeployDatabase
  jobs:
  - job: FlywayDocker
    timeoutInMinutes: 10
    pool:
      vmImage: 'ubuntu-16.04'
    container:
      image: kulmam92/flyway-azure:6.2.3
    steps:
    - script: whoami
    - script: pwd
    - script: ls -la
    - script: flyway -v
```    