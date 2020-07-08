# Create a Deployment

**A Kubernetes `deployment` is responsible for keeping a set of pods running.**

1. Use the kubectl create command to create a Deployment that manages a Pod. The Pod runs a Container based on the provided Docker image.

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

     > **Note**: wait until the READY field displays `1/1`

3. View the Pod:
    
    `kubectl get pods`{{execute}}

    Output:

    ```
    NAME                          READY     STATUS    RESTARTS   AGE
    hello-app-5f85f64f6c-5zc6p   1/1       Running   0          1m
    ```
