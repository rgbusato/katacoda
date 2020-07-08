# Create a Deployment

1. Use the `kubectl create` command to create a Deployment that manages a Pod. The Pod runs a Container based on the provided Docker image.

    `kubectl create deployment hello-app --image=gcr.io/google-samples/hello-app:1.0`{{execute}}

    Output:
    
    ```
    deployment.apps/hello-app created
    ```

2. View the Deployment:

    `kubectl get deployments`{{execute}}
    
    Output:

    ```
    NAME         READY   UP-TO-DATE   AVAILABLE   AGE
    hello-app   1/1     1            1           1m
    ```

3. View the Pod:
    
    `kubectl get pods`{{execute}}

    Output:

    ```
    NAME                          READY     STATUS    RESTARTS   AGE
    hello-app-5f76cf6ccf-br9b5   1/1       Running   0          1m
    ```
