---
title: "Deploying ENBUILD for Local Testing"
description: "Steps to deploy ENBUILD on local machine"
summary: "Steps to deploy ENBUILD on local machine for quick testing"
draft: false
menu:
  docs:
    parent: "docs/how-to-guides/"
    identifier: "deployEnbuildQuickstart"
weight: 201
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Follow these step-by-step instructions to deploy ENBUILD locally for testing.

## Prerequisites
Before you begin, ensure that you have the following prerequisites in place:
- [Docker](https://docs.docker.com/engine/install/) installed on your system

### Existing Kubernetes Cluster

Ensure that you have access to a Kubernetes cluster and obtain the KubeConfig file.

You can use [rancher-desktop](https://docs.rancherdesktop.io/getting-started/installation/) or [k3d](../../references/k3d/) to spin up a local Kubernetes cluster.

### ENBUILD Container Images

Access to the ENBUILD container images are required for this deployment.
These images are published to the VivSoft managed container reigistry on `registry.gitLab.com`.
Make sure that you have the necessary credentials to pull these images.

### Helm CLI

The [Helm](https://helm.sh/) streamlines and automates Kubernetes deployments by managing charts, enabling users to easily package, version, and deploy complex applications.

## Deployment Steps:

Following are the steps you will need to take to deploy ENBUILD to your Kubernetes cluster.

### Add ENBUILD Helm Chart Repository

To add the ENBUILD Helm chart repository, run the following command:

```bash
helm repo add vivsoft https://vivsoftorg.github.io/enbuild

"vivsoft" has been added to your repositories
```

:exclamation: **Note:** If the helm repo is already present on you machine , update it to get the latest version

```bash
❯ helm repo update vivsoft
```

### Configure ENBUILD Helm Values

Before deploying ENBUILD to the Kubernetes cluster, you will need to create a custom values.yaml file so that we can specify configurations unique to this deployment.

For local deployment however we require minimum deployment values.

:exclamation: **Note:** For more information about the complete set of ENBUILD Helm values click [here](/docs/getting-started/helm-values/)!

Refer to the [example helm input file](https://github.com/vivsoftorg/enbuild/blob/main/examples/enbuild/quick_install.yaml) for guidance.


:zap: **Note:** The `imageCredentials` section is only required if the images are not public.

### Deploy ENBUILD HELM Chart

Make sure you update the values input to reference the values you created in Step 2.
Execute the command below.

```bash
helm upgrade --install --namespace enbuild enbuild vivsoft/enbuild --create-namespace -f >path of file</quick_install.yaml

Release "enbuild" does not exist. Installing it now.
NAME: enbuild
LAST DEPLOYED: Fri Mar 22 17:37:23 2024
NAMESPACE: enbuild
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
  echo "Visit http://127.0.0.1:3000 to use your application after starting the port forward"
  kubectl --namespace enbuild port-forward svc/enbuild-enbuild-ui 3000:80
```

### Validate ENBUILD Deployment

Use the following commands to validate the ENBUILD pods are up and running.

```bash
kubectl get pods -n enbuild

NAME                                       READY   STATUS    RESTARTS         AGE
enbuild-enbuild-genai-8488c86d6f-csfmn     1/1     Running   0                76m
enbuild-enbuild-ui-56f5667d5b-4xckt        1/1     Running   0                76m
enbuild-mongodb-0                          1/1     Running   0                76m
enbuild-rabbitmq-0                         1/1     Running   0                76m
enbuild-enbuild-backend-66676f8cd8-hxtbr   1/1     Running   0                76m
enbuild-enbuild-user-b87d95b45-c79p6       1/1     Running   0                76m
enbuild-enbuild-request-7c47c6d67b-j2fnd   1/1     Running   1 (73m ago)      76m
enbuild-enbuild-ml-6f944ff759-ztdj6        1/1     Running   1 (73m ago)      76m
enbuild-rabbitmq-1                         1/1     Running   0                73m
enbuild-rabbitmq-2                         1/1     Running   0                72m
enbuild-enbuild-mq-575c965764-zcnlg        1/1     Running   18 (6m24s ago)   76m

```

:exclamation: **Note:** You might see restarts of the enbuild-enbuild-mq-\* pod until the RabbitMQ service is up and running.

**Validate the ENBUILD services are setup correctly**

```bash
kubectl get services -n enbuild

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                 AGE
enbuild-rabbitmq-headless   ClusterIP   None            <none>        4369/TCP,5672/TCP,25672/TCP,15672/TCP   80s
enbuild-mongo               ClusterIP   10.43.230.6     <none>        27017/TCP                               80s
enbuild-enbuild-user        ClusterIP   10.43.140.228   <none>        80/TCP                                  80s
enbuild-enbuild-ui          ClusterIP   10.43.110.47    <none>        80/TCP                                  80s
enbuild-enbuild-backend     ClusterIP   10.43.146.20    <none>        80/TCP                                  80s
enbuild-rabbitmq            ClusterIP   10.43.54.197    <none>        5672/TCP,4369/TCP,25672/TCP,15672/TCP   80s
```

### Access ENBUILD

Use the port forwarding command to access the ENBUILD UI using your web browser.

```bash
kubectl --namespace enbuild port-forward svc/enbuild-enbuild-ui 3000:80

Forwarding from 127.0.0.1:3000 -> 8080
Forwarding from [::1]:3000 -> 8080
```

Navigate your web browser to **http://127.0.0.1:3000**. and set the admin password.

<picture><img src="/images/deployEnbuildQuickstart/initial-login.png" alt="Screenshot of ENBUILD Login Screen"></img></picture>

After you set the initial admin password, you should see the ENBUILD home page with BigBang Catalog.

<picture><img src="/images/deployEnbuildQuickstart/enbuild_home_page_first_login.png" alt="Screenshot of ENBUILD Home Screen"></img></picture>


:zap: ***[Proceed to Configureing ENBUILD](../configuring-enbuild/)***

### Uninstall ENBUILD

Use the following command to uninstall ENBUILD from your Kubernetes cluster.

```bash
helm uninstall enbuild -n enbuild

release "enbuild" uninstalled
```
