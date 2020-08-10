# Create the Role for managing deployments

1. First we need to create a Role: 

    In this yaml file we are creating the rule that allows a user to execute several operations on Deployments, Pods and ReplicaSets (necessary for creating a Deployment), which belong to the core (expressed by "" in the yaml file), apps, and extensions API Groups:

    `cat role-deployment-manager.yaml`{{execute}}

    ```yaml
    kind: Role
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      namespace: office
      name: deployment-manager
    rules:
    - apiGroups: ["", "extensions", "apps"]
      resources: ["deployments", "replicasets", "pods"]
      verbs: ["get", "list", "watch", "create", "update", "patch", "delete"] # You can also use ["*"]
    ```

    Create the Role in the cluster using the `kubectl create` command:

    `kubectl create -f role-deployment-manager.yaml`{{execute}}

    Output:

    `role.rbac.authorization.k8s.io/deployment-manager created`

    List the Role created (optional):

    ```bash
    $ kubectl get roles -n office
    NAME                 CREATED AT
    deployment-manager   2020-08-09T20:35:29Z
    ```
