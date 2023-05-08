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

# Before we begin

## Prerequisites

Your lab environment has been pre-configured with the following:

- Azure CLI
- Azure Subscription
- Visual Studio Code
- Docker Desktop
- Git

## Workshop instructions

When you see these blocks of text, you should follow the instructions below.

<div class="task" data-title="Task">

> This means you need to perform a task.

</div>

<div class="info" data-title="Info">

> This means you should pay attention to some information.

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

# Kubernetes fundamentals

This workshop will introduce you to the basics of Kubernetes. We'll be using Azure Kubernetes Service (AKS) to deploy and manage the Azure Voting App.

## Working with `kubectl`

Kubernetes administartors will commonly interact with the Kuberetes API server using the `kubectl` command line tool. As you progress through your cloud natvie journey, you will find that there are other tools available for deploying, managing, and monitoring Kubernetes clusters. However, basic knowledge of `kubectl` is essential.

### Connecting to your AKS cluster

An AKS cluster has been provisioned for you. Let's use the Azure CLI to download the credentials for the cluster. You will need to replace `<resource-group>` and `<cluster-name>` with the values provided to you.

<div class="task" data-title="Task">

> Run the following command to download the credentials for your AKS cluster.

</div>

```bash
RG_NAME=<resource-group>
AKS_NAME=<cluster-name>
az aks get-credentials --resource-group $RG_NAME --name $AKS_NAME
```

The command above will download the credentials for the cluster and store them in `~/.kube/config`. This file is used by `kubectl` to connect to the cluster.

### Basics of `kubectl`

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

A Pod is the smallest unit of deployment in Kubernetes. It is a group of one or more containers that share the same network and storage. In this case, we are running a single container using the `nginx` image.

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

YAML is a human-readable data serialization language. It is commonly used for configuration files and in applications where data is being stored or transmitted. YAML stands for "YAML Ain't Markup Language".

<div class="task" data-title="Task">

> Next, let's deploy our Pod using the YAML file we just created. Don't worry if you don't understand the YAML file. We'll be covering that in more detail later.

</div>

```bash
kubectl apply -f nginx2.yaml
kubectl get pods
```

Here, we are telling Kubernetes that we want a Pod named `nginx2` using the `nginx` image. The deployment details are specified in a YAML file which was passed in using here-doc syntax.

This is different from the imperative approach where we told Kubernetes to run a Pod named `nginx` using the `nginx` image. The declarative approach is preferred because it allows us to check our code into source control and track changes over time.

The `kubectl apply` command is idempotent. This means that if you run the command multiple times, the result will be the same. If the resource already exists, it will be updated. If the resource does not exist, it will be created.

<div class="important" data-title="Important">

> Be sure to delete the pods so that we don't waste cluster resources.

</div>

```bash
kubectl delete pods --all
```

---

# Deploying to AKS

We'll be deploying the Azure Voting App to Azure Kubernetes Service (AKS). This is a simple app that lets you vote for things and displays the vote totals. You may recognize this app from Microsoft Docs which allows you to vote for "Dogs" or "Cats". The example we'll be using is a slightly different in that it's been modified to allow you to vote for any two things you want based on the environment variables you set.

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

> Let's start by getting the name of your ACR instance. Be sure to replace `<resource-group>` with the name of your resource group.

</div>

```bash
RG_NAME=<resource-group>

# get the name of the ACR instance
ACR_NAME=$(az resource list \
  --resource-group $RG_NAME \
  --resource-type Microsoft.ContainerRegistry/registries \
  --query "[0].name" \
  --output tsv)
```

<div class="task" data-title="Task">

> Make sure you are at the root of your repository then run the following command to build and push the image to ACR using [ACR Tasks](https://learn.microsoft.com/azure/container-registry/container-registry-tasks-overview).

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

Earlier, we talked about how Kubernetes uses YAML files to describe the state of your cluster.

In the previous secion, we used `kubectl` to run a pod using both the imperative and declarative approaches. But, did you know that `kubectl` can also be used to generate YAML files? Let's take a look at how we can do that to generate a YAML file for our app.

<div class="task" data-title="Task">

> Open a new terminal and make sure you are at the root of the repo then run the following command to generate a YAML file for our app.

</div>

```bash
# set the resource group name again since we are in a new terminal
RG_NAME=<resource-group>

# set the name of the ACR instance again since we are in a new terminal
ACR_NAME=$(az resource list \
  --resource-group $RG_NAME \
  --resource-type Microsoft.ContainerRegistry/registries \
  --query "[0].name" \
  --output tsv)

kubectl create deploy azure-voting-app \
  --image $ACR_NAME.azurecr.io/azure-voting-app:latest \
  --port=8080 \
  --dry-run=client \
  --output yaml > azure-voting-app-deployment.yaml
```

The `--dry-run=client` flag combined with the `--output yaml` flag tells `kubectl` to generate the YAML file but not actually run the command. This is useful because it allows us to see what the YAML file will look like before we actually run it. By redirecting the output to a file, we can save the YAML file to disk. If you open up the YAML file, you'll see that most of the details have been filled in for you 🥳

Notice that we are creating a **Deployment** resource instead of a **Pod** resource? This is because we want to be able to scale our app up and down. If we were to use a Pod resource, we would only be able to run a single instance of our app.

## Configuring apps

The base YAML file that was generated for us is a good starting point, but we need to make a few changes to it before we can deploy it to AKS. The first thing we need to do is add the environment variables to configure the app.

But wait, we don't know where exactly to put the environment variables in the YAML file. Never fear, `kubectl` is here!

<div class="task" data-title="Task">

> Run the following `kubectl explain` command to get more information about Deployments.

</div>

```bash
kubectl explain deploy.spec.template.spec.containers
```

Here we are using `kubectl explain` to get information about the Deployment resource. We are then drilling down into the `spec.template.spec.containers` section to get information about the `containers` property.

<div class="info" data-title="Info">

> You can traverse the through all the Deployment properties in this way to get more information about them. Additionally, you can also use `kubectl explain` to get more information about other Kubernetes resources.
>
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

> Now that we know where to put the environment variables, let's add them to the YAML file. Open the `azure-voting-app-deployment.yaml` file, place your cursor after the `resource: {}` line, and add the following YAML.

</div>

```yaml
env:
  - name: FIRST_VALUE
    value: "Dogs"
  - name: SECOND_VALUE
    value: "Cats"
```

<div class="important" data-title="Important">

> YAML is very sensitive to indentation. Make sure you indent the environment variables correctly. The resulting YAML file should look like this:

</div>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: azure-voting-app
  name: azure-voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-voting-app
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: azure-voting-app
    spec:
      containers:
        - image: acruser123.azurecr.io/azure-voting-app:latest
          name: azure-voting-app
          resources: {}
          env:
            - name: FIRST_VALUE
              value: "Dogs"
            - name: SECOND_VALUE
              value: "Cats"
status: {}
```

We also need database credentials to be able to connect to the database. We could add them to the YAML file, but that would mean that they would be stored in plain text. This is not a good idea because anyone who has access to the YAML file would be able to see the credentials. Instead, we are going to use a Kubernetes secret to store the credentials.

<div class="task" data-title="Task">

> Run the following command to create a secret for the database credentials.

</div>

```bash
kubectl create secret generic azure-voting-db-secrets \
  --from-literal=servername=azure-voting-db \
  --from-literal=password=mypassword
```

Now that we have created the secret, we need to tell Kubernetes to use it. We can do this by adding a `envFrom` property to the `containers` object.

<div class="task" data-title="Task">

> In the `azure-voting-app-deployment.yaml` file, add the following YAML to the YAML file directly below the `SECOND_VALUE` environment variable.

</div>

```yaml
- name: DATABASE_SERVER
  valueFrom:
    secretKeyRef:
      name: azure-voting-db-secrets
      key: servername
- name: DATABASE_PASSWORD
  valueFrom:
    secretKeyRef:
      name: azure-voting-db-secrets
      key: password
```

<div class="info" data-title="Info">

> Your `azure-voting-app-deployment.yaml` file should now look like this:

</div>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: azure-voting-app
  name: azure-voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-voting-app
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: azure-voting-app
    spec:
      containers:
        - image: acruser123.azurecr.io/azure-voting-app:latest
          name: azure-voting-app
          ports:
            - containerPort: 8080
          resources: {}
          env:
            - name: FIRST_VALUE
              value: "Dogs"
            - name: SECOND_VALUE
              value: "Cats"
            - name: DATABASE_SERVER
              valueFrom:
                secretKeyRef:
                  name: azure-voting-db-secrets
                  key: servername
            - name: DATABASE_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: azure-voting-db-secrets
                  key: password
status: {}
```

We're nearly there. We just need to configure the PostgreSQL database deployment. The process of creating the YAML will be very similar to what we did for the Azure Voting App deployment.

<div class="task" data-title="Task">

> Using `kubectl`, create a file called `azure-voting-db-deployment.yaml`.

</div>

```base
kubectl create deployment azure-voting-db \
  --image=postgres \
  --dry-run=client \
  -o yaml > azure-voting-db-deployment.yaml
```

<div class="task" data-title="Task">

> Open the `azure-voting-db-deployment.yaml` file and add the following YAML to it (just below the `resources` property).

</div>

```yaml
env:
  - name: POSTGRES_PASSWORD
    valueFrom:
      secretKeyRef:
        name: azure-voting-db-secrets
        key: password
```

<div class="info" data-title="Info">

> Your `azure-voting-db-deployment.yaml` file should now look like this:

</div>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: azure-voting-db
  name: azure-voting-db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-voting-db
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: azure-voting-db
    spec:
      containers:
        - image: postgres
          name: postgres
          resources: {}
          env:
            - name: POSTGRES_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: azure-voting-db-secrets
                  key: password
status: {}
```

<div class="task" data-title="Task">

> Run the following command to create the PostgreSQL database deployment.

</div>

```bash
kubectl apply -f azure-voting-db-deployment.yaml
```

Next, we need to create a service for the PostgreSQL database. This will allow the Azure Voting App to connect to the database. We can do this by using the `kubectl expose` command and specifying the deployment name. We can also use the same technique of creating a YAML file and then modifying it to suit our needs.

<div class="task" data-title="Task">

> Run the following command to create a service for the PostgreSQL database deployment.

</div>

```bash
# create the YAML file
kubectl expose deployment azure-voting-db \
  --port=5432 \
  --target-port=5432 \
  --name=azure-voting-db \
  --dry-run=client \
  -o yaml > azure-voting-db-service.yaml

# deploy the YAML file
kubectl apply -f azure-voting-db-service.yaml
```

<div class="task" data-title="Task">

> Run the following command to create a service for the Azure Voting App.

</div>

```bash
kubectl apply -f azure-voting-app-deployment.yaml
```

<div class="task" data-title="Task">

> Run the following command to create a service for the Azure Voting App deployment.

</div>

```bash
# create the YAML file
kubectl expose deployment azure-voting-app \
  --port=8080 \
  --target-port=8080 \
  --name=azure-voting-app \
  --dry-run=client \
  -o yaml > azure-voting-app-service.yaml

# deploy the YAML file
kubectl apply -f azure-voting-app-service.yaml
```

Now that we have deployed the Azure Voting App and the PostgreSQL database, we can check to see if they are running.

```bash
kubectl get deployments,pods,services
```

<div class="info" data-title="Info">

> You should see something like this:

</div>

```text
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/azure-voting-app   1/1     1            1           27m
deployment.apps/azure-voting-db    1/1     1            1           27m

NAME                                    READY   STATUS    RESTARTS   AGE
pod/azure-voting-app-6bc9446ddb-xvdgc   1/1     Running   0          10m
pod/azure-voting-db-5666f7fc58-nph78    1/1     Running   0          27m

NAME                       TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
service/azure-voting-app   ClusterIP   10.0.185.0   <none>        8080/TCP   22s
service/azure-voting-db    ClusterIP   10.0.13.23   <none>        5432/TCP   3m
service/kubernetes         ClusterIP   10.0.0.1     <none>        443/TCP    171m
```

The application and services are now running, but we can't access it yet. If you noticed, there is no way to access the application from outside the cluster. We need to expose the application to the outside world. We can do this by using the `kubectl port-forward` command for now.

<div class="task" data-title="Task">

> Run the following command to expose the application.

</div>

```bash
kubectl port-forward service/azure-voting-app 8080:8080
```

<div class="info" data-title="Info">

> Kubernetes will now forward all traffic from port 8080 on your local machine to port 8080 on the `azure-voting-app` service.

</div>

Now that we have exposed the application, we can access it from our local machine. Open a browser and go to `http://localhost:8080`. You should see the Azure Voting App.

TODO: add screenshot

---

## Dealing with secrets

### So secrets are not really secret in Kubernetes. Let's see how we can deal with that.

---

## Persistent storage

### Databases need to store data. Let's see how we can do that in Kubernetes.

---

## Sharing your app with the world

## Sharing is caring. Let's see how we can expose our app to the world.

---

## Observing your app

### Let's take a look at how our app is performing.

---

## Scaling your app

### The app is running, but it's not very useful if it can't handle a lot of traffic. Let's scale it up!

---

## Automating deployments

## We've been deploying our app manually. Let's see how we can automate that.

## Resources

- https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#define-container-environment-variables-using-secret-data

```

```
