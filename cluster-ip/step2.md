# Create Service

1. Let's create our service of type `ClusterIP`::

    `kubectl expose deployment hello-node --type=ClusterIP --port=8080`{{execute}}

    The `--type=ClusterIP` flag is not really necessary since `ClusterIP` is the **default Service type**.

    Output:

    ```
    service/hello-node exposed
    ```

    The above `kubectl expose` command is equivalent to `kubectl apply -f` the following yaml:

    ```
    apiVersion: v1
    kind: Service
    metadata:
        name: hello-node
        labels:
            app: hello-node
    spec:
        ports:
        - port: 8080
          protocol: TCP
        selector:
            app: hello-node

    ```

2. View the Service you just created:

    `kubectl get services`{{execute}}

    Output:

    ```
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    hello-node   ClusterIP      10.96.0.1       <none>        8080:30369/TCP   21s
    ```


##### DELETE ME #####

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

    The Service specification above will create a Service which targets TCP port 80 on any Pod with the `run: my-nginx` label, and expose it on an abstracted Service port (`targetPort:` is the port the container accepts traffic on, `port:` is the abstracted Service port, which can be any port other pods use to access the Service). View Service API object to see the list of supported fields in service definition. Check your Service:


3. List all services deployed in the cluster:

    `kubectl get services`{{execute}}

4. To see our specific service:

    `kubectl get svc my-nginx`{{execute}}