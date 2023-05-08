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

This workshop will introduce you to the basics of Kubernetes. We'll be using Azure Kubernetes Service (AKS) to deploy and manage the Azure Voting App.

## Working with `kubectl`

Kubernetes administartors will commonly interact with the Kuberetes API server using the `kubectl` command line tool. As you progress through your cloud natvie journey, you will find that there are other tools available for deploying, managing, and monitoring Kubernetes clusters. However, basic knowledge of `kubectl` is essential.

### Connecting to your AKS cluster

An AKS cluster has been provisioned for you. Let's use the Azure CLI to download the credentials for the cluster. You will need to replace `<resource-group>` and `<cluster-name>` with the values provided to you.

<div class="task" data-title="Task">

> Run the following command to download the credentials for your AKS cluster.

</div>

```bash
az aks get-credentials --resource-group <resource-group> --name <cluster-name>
```

The command above will download the credentials for the cluster and store them in `~/.kube/config`. This file is used by `kubectl` to connect to the cluster.

### Basics of `kubectl`

Here are some basic commands to get you started with `kubectl`.

<div class="task" data-title="Task">

> To get some basic information about your cluster, run the following command:

</div>

```bash
kubectl cluster-info
```

The `kubectl` tool allows to you to interact with a variety of Kubernetes clusters.

<div class="info" data-title="Info">

> You can see the list of clusters you have access to by running the following command:

</div>

```bash
kubectl config get-contexts
```

<div class="info" data-title="Info">

> If you have more than one context listed, you can switch between clusters by running the following command:

</div>

```bash
kubectl config use-context <context-name>
```

<div class="tip" data-title="Tip">

> Be sure to checkout the [`kubectl` Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) for a list of common commands.

</div>

## Deploying your first app

The `kubectl` command line tool allows you to interact with the Kubernetes API server in a few different ways. The two most common ways are the imperative and declarative approaches. When you use the imperative approach, you are telling Kubernetes what to do. When you use the declarative approach, you are telling Kubernetes what you want.

<div class="task" data-title="Task">

> Let's deploy our first app to Kubernetes using the imperative approach.

</div>

```bash
kubectl run nginx --image nginx
```

Here, we are telling Kubernetes to run a new Pod named `nginx` using the `nginx` image.

<div class="info" data-title="Info">

> A Pod is the smallest unit of deployment in Kubernetes. It is a group of one or more containers that share the same network and storage. In this case, we are running a single container using the `nginx` image.

</div>

<div class="task" data-title="Task">

> Let's see if our Pod is running.

</div>

```bash
kubectl get pods
```

You should see something like this:

```text
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          7s
```

<div class="task" data-title="Task">

> We can also get more information about our Pod by running the following command:

</div>

```bash
kubectl describe pod nginx
```

This command will give us a lot of information about our Pod including the events that have occurred.

Now, let's take a look at how we can deploy our app using the declarative approach.

<div class="task" data-title="Task">

> First, let's create a YAML file that describes our Pod.

</div>

```bash
cat <<EOF > nginx2.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx2
spec:
  containers:
  - name: nginx2
    image: nginx
    resources: {}
EOF
```

<div class="info" data-title="Info">

> YAML is a human-readable data serialization language. It is commonly used for configuration files and in applications where data is being stored or transmitted. YAML stands for "YAML Ain't Markup Language".

</div>

<div class="task" data-title="Task">

> Next, let's deploy our Pod using the YAML file we just created. Don't worry if you don't understand the YAML file. We'll be covering that in more detail later.

</div>

```bash
kubectl apply -f nginx2.yaml
```

Here, we are telling Kubernetes that we want a Pod named `nginx2` using the `nginx` image. The deployment details are specified in a YAML file which was passed in using here-doc syntax.

This is different from the imperative approach where we told Kubernetes to run a Pod named `nginx` using the `nginx` image. The declarative approach is preferred because it allows us to check our code into source control and track changes over time.

<div class="important" data-title="Important">

> The `kubectl apply` command is idempotent. This means that if you run the command multiple times, the result will be the same. If the resource already exists, it will be updated. If the resource does not exist, it will be created.

</div>

---

# Deploying to AKS

We'll be deploying the Azure Voting App to AKS. This is a simple app that lets you vote for things. You may recognize this app from Microsoft Docs which allows you to vote for "Dogs" or "Cats". The example we'll be using is a little different. It has been modified to allow you to vote for any two things you want based on the environment variables you set.

The repo can be found here: [Azure-Samples/azure-voting-app-rust](https://github.com/Azure-Samples/azure-voting-app-rust).

## Getting familiar with Azure Voting App

As the name suggests, the app is written in Rust and uses PostgreSQL as the backend database. We'll be using Docker to package the app up into a container image and then deploying it to AKS.

Start off by forking, then cloning the repo to your local machine. When the repo has been cloned, open it up in VS Code.

TODO: ADD SCREEN SHOTS TO FORK AND CLONE THE REPO AND OPEN USING VSCODE

<div class="task" data-title="Task">

> Before we deploy the app to AKS, let's build and run it locally to make sure everything is working as expected.

</div>

```bash
docker-compose up
```

<div class="warning" data-title="Warning">

> This command will take a few minutes to complete. The first time you run it, Docker will need to download the base images and build the app. Subsequent runs will be much faster.

</div>

Once the app is running, you should be able to access it at http://localhost:8080.

TODO: ADD SCREEN SHOT OF APP RUNNING LOCALLY

If you look at the `docker-compose.yml` file, you'll see that the app is made up of two services: `app` and `db`. The `app` service is the web front-end and the `db` service is the database. In the `app` service, you'll see that there are two environment variables defined: `FIRST_VALUE` and `SECOND_VALUE`. These are the options that will be displayed on the voting page.

## Publishing the app to Azure Container Registry

Before you can deploy our app to Kubernetes, you need to package the container image and push it to a container registry. You'll be using Azure Container Registry (ACR) for this.

There are a few different ways to push an image to ACR. We'll be using the `az acr build` command which will build the image and push it to ACR in a single command.

<div class="task" data-title="Task">

> Let's start by getting the name of your ACR instance.

</div>

```bash
ACR_NAME=$(az resource list \
  --resource-group rg-user1 \
  --resource-type Microsoft.ContainerRegistry/registries \
  --query "[0].name" \
  --output tsv)
```

<div class="task" data-title="Task">

> Now, let's build and push the image to ACR.

</div>

```bash
az acr build \
  --registry $ACR_NAME \
  --image azure-voting-app:latest \
  --file Dockerfile \
  .
```

<div class="important" data-title="Important">

> This command will take a few minutes to complete. Let's move on to the next step while it's running.

</div>

## Yo, YAML!

Earlier, we talked about how Kubernetes uses YAML files to describe the state of your cluster. Let's take a look at the YAML file for our app.

We used `kubectl` to run a pod using both the imperative and declarative approaches. But, did you know that `kubectl` can also be used to generate YAML files? Let's take a look at how we can do that.

<div class="task" data-title="Task">

> Run the following command to generate a YAML file for our app.

</div>

```bash
kubectl create deploy azure-voting-app \
  --image $ACR_NAME.azurecr.io/azure-voting-app:latest \
  --dry-run=client \
  --output yaml > azure-voting-app.yaml
```

If you open up the YAML file, you'll see that most of the details have been filled in for you 🥳

## Configuring apps

The base YAML file that was generated for us is a good starting point, but we need to make a few changes to it before we can deploy it to AKS. The first thing we need to do is add the environment variables to configure the app.

But wait, we don't know where to put the environment variables in the YAML file. Never fear, `kubectl` is here!

<div class="task" data-title="Task">

> Run the `kubectl explain` command to get more information about the YAML file.

</div>

```bash
kubectl explain deploy.spec.template.spec.containers
```

Here we are using `kubectl explain` to get information about the `Deployment` resource. We are then drilling down into the `spec.template.spec.containers` section to get information about the `containers` property.

You can also use `kubectl explain` to get more information about other Kubernetes resources.

<div class="info" data-title="Info">

> To see a list of all resources that can be explained, run the following command:

</div>

```bash
kubectl api-resources
```

<div class="task" data-title="Task">

> We can see that the `containers` object has a `env` property which is an array of environment variables. If we dig a little deeper, we can see how to define environment variables.

</div>

```bash
kubectl explain deploy.spec.template.spec.containers.env
```

<div class="task" data-title="Task">

> Now that we know where to put the environment variables, let's add them to the YAML file. Place your cursor after the `resource: {}` line and add the following YAML.

</div>

```yaml
env:
  - name: FIRST_VALUE
    value: "Dogs"
  - name: SECOND_VALUE
    value: "Cats"
```

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

```
