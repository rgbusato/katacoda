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
        type: ClusterIP
        ports:
        - port: 8080
          protocol: TCP
        selector:
            app: hello-node

    ```

    > **Note**: The Service specification above will create a Service which targets TCP port 8080 on any Pod with the `app: hello-node` label, and expose it on an abstracted Service port (`targetPort:` is the port the container accepts traffic on, `port:` is the abstracted Service port, which can be any port other pods use to access the Service). View Service API object to see the list of supported fields in service definition.

2. View the Service you just created:

    `kubectl get services`{{execute}}

    Output:

    ```
    NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
    hello-node   ClusterIP   10.103.232.158   <none>        8080/TCP   19s
    ```
