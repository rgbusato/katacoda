# Deploy a hello, world app

1. Create a Deployment using the following command:

    `kubectl run web --image=gcr.io/google-samples/hello-app:1.0 --port=8080`{{execute}}

    Output:
    `deployment.apps/web created`

2. Expose the Deployment:

    `kubectl expose deployment web --target-port=8080 --type=NodePort`{{execute}}

    Output:
    `service/web exposed`

3. Verify the Service is created and is available on a node port:

    `kubectl get service web`{{execute}}

    Output:
    ```
    NAME      TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
    web       NodePort   10.104.133.249   <none>        8080:31637/TCP   12m
    ```

4. Visit the service via NodePort:

    `minikube service web --url`{{execute}}

    Output:
    `http://172.17.0.15:31637`

    > **Note:** Katacoda environment only: at the top of the terminal panel, click the plus sign, and then click Select port to view on Host 1. Enter the NodePort, in this case 31637, and then click Display Port. 

    Output:
    ```
    Hello, world!
    Version: 1.0.0
    Hostname: web-55b8c6998d-8k564
    ```

You can now access the sample app via the Minikube IP address and NodePort. The next step lets you access the app using the Ingress resource.
