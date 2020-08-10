# Bind the Role to the employee user

1. Create a RoleBinding
    
    In this file, we are binding the deployment-manager Role to the User Account employee inside the office namespace:

    `cat rolebinding-deployment-manager.yaml`{{execute}}

    ```
    kind: RoleBinding
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      name: deployment-manager-binding
      namespace: office
    subjects:
    - kind: User
      name: employee
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: Role
      name: deployment-manager
      apiGroup: rbac.authorization.k8s.io
    ```

    Create the Role in the cluster using the `kubectl create` command:

    `kubectl create -f rolebinding-deployment-manager.yaml`{{execute}}

    Output:

    `rolebinding.rbac.authorization.k8s.io/deployment-manager-binding created`

2. View RoleBinding object

    You can view the configuration of the rolebinding by running the following command:

    `kubectl describe rolebinding/deployment-manager-binding -n office`{{execute}}
