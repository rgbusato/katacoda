# Create a Deployment

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
