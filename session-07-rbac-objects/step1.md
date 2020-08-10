
# Ensure RBAC is enable for our cluster

We will be using Minikube for this lab. Minikube should already be running with the extra necessary configuration to enable RBAC for our cluster.

1. Verify RBAC is enabled for our Minikube cluster

  There are multiple ways we can check this. One quick way is to check the arguments passed in to the apiserver pod:

  `kubectl describe pod kube-apiserver-controlplane -n kube-system`{{execute}}

  Output:

  `...`

Now that we know our cluster is running with RBAC enabled lets make use of the additional RBAC Kubernetes API objects.