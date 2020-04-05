# TODO: quick note on Service

Let's create our service of type `ClusterIP`: 

<pre class="file" data-filename="my-svc.yaml" data-target="replace">---
apiVersion: v1
kind: Service
metadata:
  name: my-internal-service
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


List all services deployed in the cluster:
`kubectl get services`{{execute}}