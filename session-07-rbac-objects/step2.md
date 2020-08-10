
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
