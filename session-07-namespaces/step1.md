# Understand the default namespace

By default, a Kubernetes cluster will instantiate a default namespace when provisioning the cluster to hold the default set of Pods, Services, and Deployments used by the cluster.

1. Inspect the available namespaces by doing the following:

    `kubectl get namespaces`{{execute}}

    Output:

    ```
    NAME                   STATUS   AGE
    default                Active   22s
    kube-node-lease        Active   23s
    kube-public            Active   23s
    kube-system            Active   24s
    kubernetes-dashboard   Active   16s
    ```