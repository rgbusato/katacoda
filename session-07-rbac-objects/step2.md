# Create a ServiceAccount

To show how we can reuse the Previously create Role called deployment-manager, we will now create a new RoleBinding but this time for a ServiceAccount.

1. Create a `robot` ServiceAccount:

    `kubectl create serviceaccount robot --namespace office`{{execute}}

    Output:
    
    `serviceaccount/robot created`

2. Create a new RoleBinding:

    `cat rolebinding-robot.yaml`{{execute}}
    
    Output:

    ```yaml
    kind: RoleBinding
    apiVersion: rbac.authorization.k8s.io/v1beta1
    metadata:
      name: robot-binding
      namespace: office
    subjects:
    - kind: ServiceAccount
      name: robot
      apiGroup: ""
    roleRef:
      kind: Role
      name: deployment-manager
      apiGroup: ""
    ```

    `kubectl create -f rolebinding-robot.yaml`{{execute}}

    Output:

    ```
    rolebinding.rbac.authorization.k8s.io/robot-binding created
    ```

3. Deploy a new Pod using the ServiceAccount:

    `cat robot-pod.yaml`{{execute}}

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
        name: robot
        namespace: office
    spec:
        serviceAccountName: robot
        containers:
        - name: kubectl
            image: bitnami/kubectl:latest
            command: ['/bin/bash', '-c', 'echo "Hello, Kubernetes!" && sleep 3600']
    ```

    `kubectl create -f robot-pod.yaml`{{execute}}

    Output:

    ```bash
    controlplane $ kubectl get pods -n office
    NAME         READY   STATUS    RESTARTS   AGE
    robot        1/1     Running   0          9s
    mydokuwiki   1/1     Running   0          6m27s
    ```

4. Verify the Pod is running with the service account we've created for it:

    `kubectl get pod/robot -n office -o yaml`{{execute}}

    OR

    `kubectl describe pod/robot -n office`{{execute}}

    NOTE: even after deleting the RoleBinding pod still able to view all pods using the serviceaccount. weird

    TODO: need to audit what is actually being used

    Now we get inside the running container and try to list pods within our namespace. This exercise will make sure that the Pod (using the ServiceAccount we've created for it) can in fact view the running pod within our office namespace.

    ```bash
    kubectl exec --namespace=office -it robot -- /bin/bash
    ```

    ```bash
    I have no name!@my-pod:/$ kubectl get pods -n office
    NAME         READY   STATUS    RESTARTS   AGE
    robot        1/1     Running   0          99s
    mydokuwiki   1/1     Running   0          33m
    ```
    
    OR

    ```bash
    kubectl exec --namespace=office -it robot -- kubectl get pods -n office
    ```

    Output:

    ```
    NAME         READY   STATUS    RESTARTS   AGE
    my-pod       1/1     Running   0          2m57s
    mydokuwiki   1/1     Running   0          35m
    ```

    NOTE: why can I still see everything that is in the other namespaces ?
    Seems weird, maybe we forgot to do something here.
