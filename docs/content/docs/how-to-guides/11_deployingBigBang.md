---
title: "Deploying Big Bang Catalog Item from ENBUILD"
description: "Deploying Big Bang Catalog Item from ENBUILD"
summary: "Deploying Big Bang Catalog Item from ENBUILD"
draft: false
menu:
  docs:
    parent: "docs/how-to-guides/"
    identifier: "deployingBigbang"
weight: 215
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Follow these step-by-step instructions to deploy Big Bang.

## Prerequisites

### Existing Kubernetes Cluster

Ensure that you have access to a Kubernetes cluster and obtain the KubeConfig file.

You can use [rancher-desktop](https://docs.rancherdesktop.io/getting-started/installation/) or [k3d](../../references/k3d/) to spin up a local Kubernetes cluster.

### Platform One Iron Bank Big Bang Container Images

TBD

### SOPs CLI

The [SOPs](https://helm.sh/) streamlines and automates Kubernetes deployments by managing charts, enabling users to easily package, version, and deploy complex applications.

## Deployment Steps:

Following are the steps you will need to take to deploy Big Bang from ENBUILD.

### Login to ENBUILD

To add the ENBUILD Helm chart repository, run the following command:
