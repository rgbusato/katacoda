# Create a Service

By default, the Pod is only accessible by its internal IP address within the Kubernetes cluster. To make the `hello-app` Container accessible from outside the Kubernetes virtual network, you have to expose the Pod as a Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

1. Expose the Pod to the public internet using the `kubectl expose` command:

    `kubectl expose deployment hello-app --type=LoadBalancer --port=8080`{{execute}}

    The `--type=LoadBalancer` flag indicates that you want to expose your Service outside of the cluster.

    Output:

    ```
    service/hello-node exposed
    ```

2. View the Service you just created:

    `kubectl get services`{{execute}}

    Output:

    ```
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    hello-node   LoadBalancer   10.108.144.78   <pending>     8080:30369/TCP   21s
    ```

    > **Note:** On cloud providers that support load balancers, an external IP address would be provisioned to access the Service. **On Minikube, the LoadBalancer type makes the Service accessible through the `minikube service` command.**

3. Run the following command:

    `minikube service hello-node`{{execute}}

4. Katacoda environment only: 

    * **Note the 5 digit port number** displayed opposite to `8080` in services output. This port number is randomly generated and it can be different for you. 
    * **Click the plus sign** next to your `Terminal`, and then click **Select port to view on Host 1**
    * **Type your number in the port number text box**, then click Display Port. 
        * Using the example from earlier, you would type `30369`.
    
    > This opens up a browser window that serves your app and shows the “Hello World!” message.


