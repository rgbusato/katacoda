# Create Service

Let's create our service of type `ClusterIP`: 

`kubectl expose deployment/hello-node `




1. Expose the Pod to the public internet using the kubectl expose command:

    `kubectl expose deployment hello-node --type=ClusterIP `{{execute}}

    The `--type=ClusterIP` flag is not really necessary since ClusterIP is the default Service type.

    Output:

    ```
    service/my-nginx exposed
    ```

    This is equivalent to `kubectl apply -f` the following yaml:

    ```
    apiVersion: v1
    kind: Service
    metadata:
        name: hello-node
        labels:
            run: hello-node
    spec:
        ports:
        - port: 8080
          protocol: TCP
        selector:
            run: hello-node

    ```

2. View the Service you just created:

    `kubectl get services`{{execute}}

    The output is similar to:
    ```
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    hello-node   LoadBalancer   10.108.144.78   <pending>     8080:30369/TCP   21s
    kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          23m
    ```




Create a file named `service.yaml` with the following content:
    
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
          port: 8080
          protocol: TCP
    </pre>

2. Now let's create our service:

    `kubectl apply -f ./service.yaml`{{execute}}

    The Service specification above will create a Service which targets TCP port 80 on any Pod with the `run: my-nginx` label, and expose it on an abstracted Service port (targetPort: is the port the container accepts traffic on, port: is the abstracted Service port, which can be any port other pods use to access the Service). View Service API object to see the list of supported fields in service definition. Check your Service:


3. List all services deployed in the cluster:

    `kubectl get services`{{execute}}

4. To see our specific service:

    `kubectl get svc my-nginx`{{execute}}