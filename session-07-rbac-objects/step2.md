
# Create Namespace

1. Create Namespace

  Execute the kubectl create command to create the namespace (as the admin user):

  `kubectl create namespace office`{{execute}}

  Output:

  `namespace/office created`


2. Create the user credentials


# Step 5 - Test RBAC rule for the User
```
kubectl --context=employee-context run --image bitnami/dokuwiki mydokuwiki
kubectl --context=employee-context get pods
```


The following command will fail:

`kubectl --context=employee-context get pods --namespace=default`{{execute}}

Output:

```
Error from server (Forbidden): pods is forbidden: User "employee" cannot list resource "pods" in API group "" in the namespace "default"
```

**Now you have created a user with limited permissions in your cluster.**


-------

# test the pod permissions - end of this SO tread: https://stackoverflow.com/questions/42642170/how-to-run-kubectl-commands-inside-a-container


```
kubectl --context=employee-context run --image bitnami/dokuwiki mydokuwiki

kubectl exec -it <your-container-with-the-attached-privs> -- /kubectl get pods -n <YOUR_NAMESPACE>
kubectl run curl --rm --image=radial/busyboxplus:curl -i --tty 
```

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
