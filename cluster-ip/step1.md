NOTE: Maybe this should be the editor + terminal layout so we can create the custom service definition.

First we must deploy our Application. We will use a Deployment that will ensure that at least 2 application pods are running.
# TODO: create App deployment here to be leveraged in the next step when creating the service.

<pre class="file" data-filename="deployment.yaml" data-target="clipboard">---
apiVersion: v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  selector:
    app: my-app
  type: ClusterIP
  ports:
  - name: http
    port: 80
    targetPort: 80
    protocol: TCP
</pre>

`kubectl apply -f ./deployment.yaml`{{execute}}

List Kubernetes Deployments in the cluster:
`kubectl get deployments`{{execute}}


