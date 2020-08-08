# Namespaces Walkthrough

*This example demonstrates how to use Kubernetes namespaces to subdivide your cluster.*

Kubernetes *[namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces) help* different projects, teams, or customers to *share a Kubernetes cluster*.

It does this by providing the following:
* A scope for [Names](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/).
* A mechanism to attach authorization and policy to a subsection of the cluster.

Use of multiple namespaces is optional.

# Prerequisites
This example assumes that you have a basic understanding of Kubernetes [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) , [Services](https://kubernetes.io/docs/concepts/services-networking/service/) , and [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

Let's get started!
