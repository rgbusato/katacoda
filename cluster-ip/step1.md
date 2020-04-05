# Create a Deployment
**A Kubernetes `deployment` is responsible for keeping a set of pods running.**

1. Use the kubectl create command to create a Deployment that manages a Pod. The Pod runs a Container based on the provided Docker image.

    `kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node`{{execute}}

2. View the Deployment:

    `kubectl get deployments`{{execute}}
    
    The output is similar to:
    ```
    NAME         READY   UP-TO-DATE   AVAILABLE   AGE
    hello-node   1/1     1            1           1m
    ```

3. View the Pod:
    
    `kubectl get pods`{{execute}}

    The output is similar to:
    ```
    NAME                          READY     STATUS    RESTARTS   AGE
    hello-node-5f76cf6ccf-br9b5   1/1       Running   0          1m
    ```


## ENDS HERE

## OLD

First we must deploy our Application. We will use a Deployment that will ensure that at least 2 application pods are running.

1. Create a file named `deployment.yaml` with the following content:
<pre class="file" data-filename="deployment.yaml" data-target="replace">---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 2
  template:
    metadata:
      labels:
        run: my-nginx
    spec:
      containers:
      - name: my-nginx
        image: nginx
        ports:
        - containerPort: 80
</pre>

2. Create deployment:
`kubectl apply -f ./deployment.yaml`{{execute}}

3. List Kubernetes deployments:
`kubectl get deployments`{{execute}}

4. List pods created by our deployment:
`kubectl get pods -l run=my-nginx -o wide`{{execute}}

