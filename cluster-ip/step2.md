# Create Service

Let's create our service of type `ClusterIP`: 

**TODO:** Fix file and create file automatically?

1. Create a file named `service.yaml` with the following content:
    
    `touch service.yaml`{{execute}}

    <pre class="file" data-filename="service.yaml" data-target="replace">---
    apiVersion: v1
    kind: Service
    metadata:
    name: my-nginx
    labels:
        run: my-nginx
    spec:
        selector:
            run: my-nginx
        type: ClusterIP
        ports:
        - name: http
          port: 80
          targetPort: 80
          protocol: TCP
    </pre>

2. Now let's create our service:

    `kubectl apply -f ./service.yaml`{{execute}}

    The Service specification above will create a Service which targets TCP port 80 on any Pod with the `run: my-nginx` label, and expose it on an abstracted Service port (targetPort: is the port the container accepts traffic on, port: is the abstracted Service port, which can be any port other pods use to access the Service). View Service API object to see the list of supported fields in service definition. Check your Service:


3. List all services deployed in the cluster:

    `kubectl get services`{{execute}}

4. To see our specific service:

    `kubectl get svc my-nginx`{{execute}}