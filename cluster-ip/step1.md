NOTE: Maybe this should be the editor + terminal layout so we can create the custom service definition.

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