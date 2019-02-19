---
title: "azure down"
date: 2019-02-04T11:40:11+02:00
image: "/images/blog/azure-down.png"
tags: ["azure","database"]
comments: true
---

Databases losing data is bad. Not [once](https://www.jamesserra.com/archive/2018/08/azure-sql-database-disaster-recovery/) but [repeatedly](https://nakedsecurity.sophos.com/2019/02/01/dns-outage-turns-tables-on-azure-database-users/). 

Azure has come a long way since its rough beginnings. It was frankly unusable in its early days. To its credit, Microsoft has embraced much of the Open Source innovations and adopted aggressive improvements in Azure architecture and things have gotten much better.  Azure is usable, somewhat.  Yet, problems remain. Data loss is rather unforgivable and they occur repeatedly.

AKS (Azure Kubernetes) is still using old 1.9.X versions which are not secure. The 1.9.X version reached End-of-life as of September, 2018. The AKS is not available in AzureStack either. It is telling in a way.  Often you can only create Kubernetes cluster in the US-central.  Many resources are created and billed when you create AKS cluster. A lot of the objects and resources billed for a AKS cluster makes little sense. It is costly and slow. 

Better option is to use [Digital Ocean](https://m.do.co/c/1c6de959e2b7).  They bill monthly on flat rate.  It's [easy](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/) to create a kubernetes cluster via kubeadm.

It is easy enough to create viable k8s clusters, it does not make much sense to subscribe to the canned deployments via AKS, EKS, GKE at extraordinary markups and suspicious extra charges.

To create a  kubernetes cluster on digital ocean, you can use their kubernetes cluster service or just create some VMs. I typically use $5 per month VMs. [Here](https://5pi.de/2016/11/20/15-producation-grade-kubernetes-cluster/), you can find a good blog article on how to set up a $15 kubernetes cluster on Digital Ocean.  Here's a short version (what I typically do): On the first VM you can run the kubernetes control plane.  This master node can be tainted to allow running pods as well. The details are [here](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/).  

But basically you have 1,2,3 steps.  First, you run `kubeadm init` on the master node.  It takes up to 10 minutes. After that, you will run a CNI.  A good choice is Calico.  Run the `kubectl apply` as described in the kubeadm page.  Then go to other VM nodes and run `kubeadm join` with the printed token when `kubeadm init` was run on master.  Do this on each node.  After that you should be able to `kubectl get nodes` and see the cluster nodes.  This my preferred way to deploy kubernetes in the cloud. It is cheaper, more efficient and easier to manage than EKS, AKS or GKE.

