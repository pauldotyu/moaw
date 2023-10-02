---
published: false # Optional. Set to true to publish the workshop (default: false)
type: workshop # Required.
title: Build intelligent applications using Azure OpenAI and integrate with Azure Kubernetes Service (AKS) # Required. Full title of the workshop
short_title: Build intelligent apps with Azure OpenAI and AKS # Optional. Short title displayed in the header
description: This is a workshop that will show you how to get started in building intelligent applications using Azure OpenAI and integrating with Azure Kubernetes Service (AKS) # Required.
level: beginner # Required. Can be 'beginner', 'intermediate' or 'advanced'
authors: # Required. You can add as many authors as needed
  - "Paul Yu"
contacts: # Required. Must match the number of authors
  - "@pauldotyu"
duration_minutes: 75 # Required. Estimated duration in minutes
tags: kubernetes, azure, openai, intelligent-apps # Required. Tags for 
wt_id: WT.mc_id=containers-107692-pauyu
---

# Build intelligent apps with Azure OpenAI and AKS

In this workshop, you will learn how to build intelligent applications using Azure OpenAI and integrate with Azure Kubernetes Service (AKS). You will learn how to deploy an application to AKS, deploy an Azure OpenAI model, and configure the application to securely access Azure OpenAI. The application will be a simple store application that will use Azure OpenAI to generate product descriptions.

## Objectives

The objectives of this workshop are to:

- Deploy an application to Azure Kubernetes Service
- Deploying models to Azure OpenAI
- Configuring an application to securely access Azure OpenAI

## Prerequisites

The lab environment was pre-configured with the following:

- [Azure Subscription](https://azure.microsoft.com/free)
- [Azure CLI](https://learn.microsoft.com/cli/azure/what-is-azure-cli?WT.mc_id=containers-105184-pauyu)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Git](https://git-scm.com/)
- Bash shell (e.g. [Windows Terminal](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701) with [WSL](https://docs.microsoft.com/windows/wsl/install-win10) or [Azure Cloud Shell](https://shell.azure.com))

## Workshop instructions

When you see these blocks of text, you should follow the instructions below.

<div class="task" data-title="Task">

> This means you need to perform a task.

</div>

<div class="info" data-title="Info">

> This means there's some additional context.

</div>

<div class="tip" data-title="Tip">

> This means you should pay attention to some helpful tips.

</div>

<div class="warning" data-title="Warning">

> This means you should pay attention to some information.

</div>

<div class="important" data-title="Important">

> This means you should **_really_** pay attention to some information.

</div>

---

## Deploy an intelligent app to AKS

### Deploy and test the demo app

### Deploy an OpenAI model

### Add AI to demo app

### Test the AI-enabled demo app

---

## Secure access to Azure OpenAI from AKS

### Overview of EntraID Workload Identity

#### User-Managed Identity

#### Azure Role-Based Access Control

#### Federated Identity Credential

### Create Kubernetes Service Account

### Create Kubernetes ConfigMap

### Update Kubernetes Deployment

### Validate secure access

---

## Summary

### Additional Resources
