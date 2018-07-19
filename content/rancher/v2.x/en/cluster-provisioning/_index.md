---
title: Provisioning Kubernetes Clusters
weight: 2000
aliases:
  - /rancher/v2.x/en/concepts/clusters/
  - /rancher/v2.x/en/concepts/clusters/cluster-providers/
  - /rancher/v2.x/en/tasks/clusters/
  - /rancher/v2.x/en/tasks/clusters/creating-a-cluster/
---

## What's a Kubernetes Cluster?

In the IT world, a cluster is a group of computing resources that work as a team to accomplish a goal.

A _Kubernetes Cluster_ is a cluster that uses the [Kubernetes container-orchestration system](https://kubernetes.io/) to deploy, maintain, and scale Docker containers, allowing your organization to automate application operations. Kubernetes reduces the manual processes of maintaining organization operations.

### Kubernetes Cluster Node Components

Each computing resource in a Kubernetes Cluster is called a _node_. Node can be either bare-metal servers or virtual machines. Kubernetes classifies nodes into three distinct types: _etcd_ nodes, _control plane_ nodes, and _worker_ nodes. Understanding the role of each node will help you create your own Kubernetes cluster.

#### etcd Nodes

[etcd](https://kubernetes.io/docs/concepts/overview/components/#etcd) nodes run the `etcd` database. The `etcd` database component is a key value store used as Kubernetes storage for all cluster data, such as cluster coordination and state management.

`etcd` is a distributed key value store, meaning it runs on multiple nodes so that there's always a backup available for fail over. Even though you can run `etcd` on a single node, you should run it on multiple nodes. We recommend 3, 5, or 7 nodes for redundancy.

#### Control Plane Nodes

[Control plane](https://kubernetes.io/docs/concepts/#kubernetes-control-plane) nodes run the Kubernetes API server, scheduler, and controller manager. These nodes take care of routine tasks to ensure that your Kubernetes cluster is running according to your configuration. Because all cluster data is stored on your `etcd` nodes, control plane nodes are stateless. You can run control plane on a single node, although two or more nodes are recommended for redundancy. Additionally, you can a single node can share the control plane and `etcd` roles.

#### Worker Nodes

[Worker nodes](https://kubernetes.io/docs/concepts/architecture/nodes/) run:

- _Kubelets_: An agent that monitors the state of the node, ensuring your containers are healthy.
- _Workloads_: The containers and pods that hold your apps, as well as other types of deployments.

Worker nodes also run storage and networking drivers, and ingress controllers when required. You create as many worker nodes as needed for your workload needs.

## Cluster Creation in Rancher

Now that you know what a Kubernetes Cluster is, how does Rancher fit in?

Rancher simplifies creation of Kubernetes clusters by allowing you to create them through the Rancher UI rather than more complex alternatives. Rancher provides multiple options for launching a Kubernetes cluster. Use the option that best fits you use case.

## Cluster Creation Options

Options include:

<!-- TOC -->

- [Hosted Kubernetes Cluster](#hosted-kubernetes-cluster)
- [Rancher Launched Kubernetes](#rancher-launched-kubernetes)

    - [Node Pools](#node-pools)
    - [Custom Nodes](#custom-nodes)
- [Import Existing Cluster](#import-existing-cluster)

<!-- /TOC -->

### Hosted Kubernetes Cluster

If you already use a Kubernetes provider such as Google GKE, Rancher can integrate with its cloud APIs, allowing you to create and manage your hosted cluster from the Rancher UI.

[Hosted Kubernetes Cluster]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/hosted-kubernetes-clusters)

### Rancher Launched Kubernetes

Alternatively, you can use Rancher to create the Kubernetes cluster on your own nodes, using [Rancher Kubernetes Engine (RKE)]({{< baseurl >}}/rke/v0.1.x/en/). RKE is Rancher’s own lightweight Kubernetes installer. With these clusters, Rancher manages the deployment of Kubernetes. These Kubernetes clusters can be deployed on any bare metal server, cloud provider, or virtualization platform. These nodes can either be provisioned through Rancher's UI, which calls [Docker Machine](https://docs.docker.com/machine/) to launch nodes on various cloud providers or they can be existing nodes that users bring and run a Rancher agent container onto.

[Rancher Launched Kubernetes]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/)

#### Node Pools

Using Rancher, you can create pools of nodes based on a [node template]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/#node-templates). This node template defines the parameters you want to use to launch nodes in your cloud providers. The available cloud providers to create a node template are decided based on active [node drivers]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/#node-drivers). The benefit of using a node pool is that if a node loses connectivity with the cluster, Rancher will automatically create another node to join the cluster to ensure that the count of the node pool is as expected.

[Node Pools]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/)

#### Custom Nodes

You can bring any nodes you want to Rancher and have Rancher create the Kubernetes cluster. These nodes can include on-premise bare metal servers, nodes existing in a cloud provider or virtual machines.

[Custom Nodes]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/custom-nodes/)

### Import Existing Cluster

Users can import an existing Kubernetes cluster into Rancher. Rancher does not automate the provisioning, scaling, and upgrade of imported Kubernetes clusters. All other cluster management, policy management, and workload management capabilities of Rancher apply to imported clusters.

[Importing Existing Cluster]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/imported-clusters/)
