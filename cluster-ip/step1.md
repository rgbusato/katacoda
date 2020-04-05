
**A Kubernetes `deployment` is responsible for keeping a set of pods running.**

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

