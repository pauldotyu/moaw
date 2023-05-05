---
published: false # Optional. Set to true to publish the workshop (default: false)
type: workshop # Required.
title: Microsoft Build PRE03 # Required. Full title of the workshop
short_title: Getting Started with AKS # Optional. Short title displayed in the header
description: This is a workshop for AKS # Required.
level: beginner # Required. Can be 'beginner', 'intermediate' or 'advanced'
authors: # Required. You can add as many authors as needed
  - Paul Yu
contacts: # Required. Must match the number of authors
  - "@pauldotyu"
duration_minutes: 20 # Required. Estimated duration in minutes
tags: kubernetes, azure # Required. Tags for filtering and searching
#banner_url: assets/banner.jpg           # Optional. Should be a 1280x640px image
#video_url: https://youtube.com/link     # Optional. Link to a video of the workshop
#audience: students                      # Optional. Audience of the workshop (students, pro devs, etc.)
#wt_id: <cxa_tracking_id>                # Optional. Set advocacy tracking code for supported links
#oc_id: <marketing_tracking_id>          # Optional. Set marketing tracking code for supported links
#navigation_levels: 2                    # Optional. Number of levels displayed in the side menu (default: 2)
#sections_title:                         # Optional. Override titles for each section to be displayed in the side bar
#   - Section 1 title
#   - Section 2 title
---

# Kubernetes Fundamentals

This workshop will introduce you to the basics of Kubernetes. We'll be using Azure Kubernetes Service (AKS) to deploy and manage our applications.

## Working with `kubectl`

You work with Kubernetes using its API and this API can be accessed using the `kubectl` command line tool. We'll be using this tool to interact with our Kubernetes cluster.

### Connecting to an AKS cluster

We have already created an AKS cluster for you. You can connect to it using the following command:

```bash
az aks get-credentials --resource-group <resource-group> --name <cluster-name>
```

### Basics of `kubectl`

<div class="tip" data-title="Tip">

> Highly recommend you implemnet terminal settings as found in the [`kubectl` Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

</div>

```bash
kubectl cluster-info
kubectl config get-contexts
kubectl config use-context <context>
```

## Deploying your first app

Let's deploy our first app to Kubernetes using the imperative approach.

```bash
kubectl run nginx --image nginx
kubectl port-forward po/nginx 8080:80
```

```

---

## Deploying to AKS

We'll be deploying the Azure Voting App to AKS. This is a simple app that lets you vote for your favorite `<insert whatever you want here>`.

## But first, Docker

Before we can deploy our app to Kubernetes, we need to package it up into a Docker image. Let's do that first.

## Yay, YAML!

Who doesn't love YAML? We'll be using YAML to describe our Kubernetes resources.

---

## Configuring apps

Customizing your app with environment variables. 12-factor apps.

---

## Dealing with secrets

So secrets are not really secret in Kubernetes. Let's see how we can deal with that.

---

## Persistent storage

Databases need to store data. Let's see how we can do that in Kubernetes.

---

## Sharing your app with the world

Sharing is caring. Let's see how we can expose our app to the world.

---

## Observing your app

Let's take a look at how our app is performing.

---

## Scaling your app

The app is running, but it's not very useful if it can't handle a lot of traffic. Let's scale it up!

---

## Automating deployments

We've been deploying our app manually. Let's see how we can automate that.
```
