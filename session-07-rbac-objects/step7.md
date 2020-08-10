# Create a ServiceAccount

Imagine a situation where an SRE team member called `employee` was manually managing deployments to our cluster for the `office` namespace. 

We finally decided that we would like to automate this process by running a Pod within the cluster that automatically performs deployments within the the `office` namespace.

Since Pods use ServiceAccounts, we need to start by creating a ServiceAccount that our deployment automation Pod will use to communicate with the Kubernetes API and manage our deployments within the namespace.

1. Create a `robot` ServiceAccount:
   
    `kubectl create serviceaccount robot --namespace office`{{execute}}

    Output:
    
    `serviceaccount/robot created`

2. Create a new RoleBinding:
    
    To show how we can reuse the previously create Role called `deployment-manager`, we will now create a new RoleBinding but this time for a ServiceAccount that we have just created above

    `cat rolebinding-robot.yaml`{{execute}}
    
    Output:

    ```yaml
    kind: RoleBinding
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      name: robot-binding
      namespace: office
    subjects:
    - kind: ServiceAccount
      name: robot
    roleRef:
      kind: Role
      name: deployment-manager # we are reusing the previously created Role
      apiGroup: rbac.authorization.k8s.io
    ```

    `kubectl create -f rolebinding-robot.yaml`{{execute}}

    Output:

    ```
    rolebinding.rbac.authorization.k8s.io/robot-binding created
    ```

3. Deploy a new Pod using the ServiceAccount:

    Let's create a new Pod that uses our newly created ServiceAccount to run:

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
    $ kubectl get pods -n office
    NAME         READY   STATUS    RESTARTS   AGE
    robot        1/1     Running   0          9s
    mydokuwiki   1/1     Running   0          6m27s
    ```

4. Verify the Pod is running with the service account we've created for it:

    `kubectl get pod/robot -n office -o yaml`{{execute}}

    OR

    `kubectl describe pod/robot -n office`{{execute}}

5. Run kubectl within the Pod to test it's access:

    Now we get inside the running container and try to list pods within our namespace. 
    
    This exercise will make sure that the Pod (using the ServiceAccount we've created for it) can in fact view the running pod within our office namespace.

    > **Note:** The following command attaches to the existing Pod named `robot` and run `kubectl get pods -n office` within the existing pod. This is how we test our ServiceAccount access.

    `kubectl exec --namespace=office -it robot -- kubectl get pods -n office`{{execute}}

    Output:

    ```bash
    NAME         READY   STATUS    RESTARTS   AGE
    robot        1/1     Running   0          2m57s
    mydokuwiki   1/1     Running   0          35m
    ```

    That's great, but can I access resources in other namespaces?

    `kubectl exec --namespace=office -it robot -- kubectl get pods -n default`{{execute}}

    Output:

    ```bash
    Error from server (Forbidden): pods is forbidden: User "system:serviceaccount:office:robot" cannot list resource "pods" in API group "" in the namespace "default"
    command terminated with exit code 1
    ```

    Apparently not, that is exactly what we wanted!
