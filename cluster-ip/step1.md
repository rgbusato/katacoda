NOTE: Maybe this should be the editor + terminal layout so we can create the custom service definition.

First we must deploy our Application. We will use a Deployment that will ensure that at least 2 application pods are running.
# TODO: create App deployment here to be leveraged in the next step when creating the service.

<pre class="file" data-filename="deployment.yaml" data-target="clipboard">
---
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

`kubectl apply -f ./deployment.yaml`{{execute}}

List Kubernetes Deployments in the cluster:
`kubectl get deployments`{{execute}}

List pods created by out Deployment:
`kubectl get pods -l run=my-nginx -o wide`{{execute}}

