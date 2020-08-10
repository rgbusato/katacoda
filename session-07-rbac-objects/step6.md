
# Test the RBAC rule

1. Now you should be able to execute the following commands without any issues:

    ```
    kubectl --context=employee-context run --image bitnami/dokuwiki mydokuwiki
    kubectl --context=employee-context get pods
    ```{{execute}}

    Output:

    `...`

2. The user should not have access to the `default` namespace:
   
    If you run the same command with the `--namespace=default` argument, it will fail, as the employee user does not have access to this namespace.


    The following command will fail:

    `kubectl --context=employee-context get pods --namespace=default`{{execute}}

    Output:

    ```
    Error from server (Forbidden): pods is forbidden: User "employee" cannot list resource "pods" in API group "" in the namespace "default"
    ```

    **Now you have created a user with limited permissions in your cluster.**


# Create a POD (will use the default serviceaccount) 

`kubectl run --namespace office --image bitnami/dokuwiki mydokuwiki`

`kubectl get pods -n office`

# Create a new POD (will use the specific ServiceAccount we've created)

`cat robot-pod.yaml`{{execute}}
```
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

```
controlplane $ kubectl get pods -n office
NAME         READY   STATUS    RESTARTS   AGE
robot        1/1     Running   0          9s
mydokuwiki   1/1     Running   0          6m27s
```

Verify the pod is running with the service account we've created for it:
`kubectl get pod/robot -n office -o yaml`{{execute}}

----
```
...
# service account info
serviceAccount: robot
serviceAccountName: robot
...
# robot serviceaccount token mounted to the pod
volumes:
  - name: robot-token-4brqx
    secret:
      defaultMode: 420
      secretName: robot-token-4brqx
```
