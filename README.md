# Azure Pipelines Agent

[Azure Pipelines Agent](https://github.com/emberstack/docker-azure-pipelines-agent) is self-hosted agent that you can run in a container with Docker.

[![Pipeline](https://github.com/emberstack/docker-azure-pipelines-agent/actions/workflows/pipeline.yaml/badge.svg)](https://github.com/emberstack/docker-azure-pipelines-agent/actions/workflows/pipeline.yaml)
[![Release](https://img.shields.io/github/release/emberstack/docker-azure-pipelines-agent.svg?style=flat-square)](https://github.com/emberstack/docker-azure-pipelines-agent/releases/latest)
[![Docker Image](https://img.shields.io/docker/image-size/emberstack/azure-pipelines-agent/latest?style=flat-square)](https://hub.docker.com/r/emberstack/azure-pipelines-agent)
[![Docker Pulls](https://img.shields.io/docker/pulls/emberstack/azure-pipelines-agent.svg?style=flat-square)](https://hub.docker.com/r/emberstack/azure-pipelines-agent)
[![license](https://img.shields.io/github/license/emberstack/docker-azure-pipelines-agent.svg?style=flat-square)](LICENSE)

> Supports `amd64`, `arm` and `arm64`

## Agent Version

This image will automatically pull and install the latest Azure DevOps version at startup.

### Support

If you need help or found a bug, please feel free to open an issue on the [emberstack/docker-azure-pipelines-agent](https://github.com/emberstack/docker-azure-pipelines-agent) GitHub project.

## Deployment

The Azure Pipeliens agent can be deployed in Docker using either `docker run` or `docker compose` or in Kubernetes using Helm (recommended).

#### Deployment in `docker`

```
docker run -d -e AZP_AGENT_NAME="<agent name>" -e AZP_URL="https://dev.azure.com/<your org.>" -e AZP_POOL="<agent pool>" -e AZP_TOKEN="<PAT>" emberstack/azure-pipelines-agent
```

#### Deployment in `Kubernetes`

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: tools-server
    run: az-agent
  name: az-agent
  namespace: tools
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tools-server
      run: az-agent
  template:
    metadata:
      labels:
        app: tools-server
        run: az-agent
    spec:
      containers:
        - name: az-agent
          image: rafasousa/azure-pipelines-agent
          env:
            - name: AZP_AGENT_NAME
              value: "<agent name>"
            - name: AZP_URL
              value: "https://dev.azure.com/<your org.>"
            - name: AZP_POOL
              value: "<agent pool>"
            - name: AZP_TOKEN
              value: "<PAT>"
```
