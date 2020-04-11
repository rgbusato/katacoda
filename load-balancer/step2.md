# Create a Service

By default, the Pod is only accessible by its internal IP address within the Kubernetes cluster. To make the `hello-node` Container accessible from outside the Kubernetes virtual network, you have to expose the Pod as a Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

1. Expose the Pod to the public internet using the `kubectl expose` command:

    `kubectl expose deployment hello-node --type=LoadBalancer --port=8080`{{execute}}

    The `--type=LoadBalancer` flag indicates that you want to expose your Service outside of the cluster.

2. View the Service you've just created:

    `kubectl get services`{{execute}}

    The output is similar to:
    ```
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    hello-node   LoadBalancer   10.108.144.78   <pending>     8080:30369/TCP   21s
    kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          23m
    ```

    > **Note:** On cloud providers that support load balancers, an external IP address would be provisioned to access the Service. On Minikube, the LoadBalancer type makes the Service accessible through the minikube service command.

3. Run the following command:

    `minikube service hello-node`{{execute}}

4. Katacoda environment only: Click the plus sign, and then click Select port to view on Host 1

5. Katacoda environment only: Note the 5 digit port number displayed opposite to 8080 in services output. This port number is randomly generated and it can be different for you. Type your number in the port number text box, then click Display Port. Using the example from earlier, you would type 30369.
    This opens up a browser window that serves your app and shows the “Hello World” message.


