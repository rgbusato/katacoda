
# Test the RBAC rule

1. Now you should be able to execute the following commands without any issues:

    ```
    kubectl --context=employee-context run --image bitnami/dokuwiki mydokuwiki
    kubectl --context=employee-context get pods
    ```{{execute}}

    Output:

    ```bash
    NAME                          READY   STATUS              RESTARTS   AGE
    mydokuwiki-5986fdc457-988h4   0/1     ContainerCreating   0          0s
    ```

2. The user should not have access to the `default` namespace:
   
    If you run the same command with the `--namespace=default` argument, it will fail, as the employee user does not have access to this namespace.

    The following command **will FAIL**:

    `kubectl --context=employee-context get pods --namespace=default`{{execute}}

    Output:

    ```
    Error from server (Forbidden): pods is forbidden: User "employee" cannot list resource "pods" in API group "" in the namespace "default"
    ```

    **Now you have created a user with limited permissions in your cluster.**
