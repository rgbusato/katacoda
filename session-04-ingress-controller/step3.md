# Create an Ingress resource
The following file is an Ingress resource that sends traffic to your Service via hello-world.info.

1. Take a look at the contents of `example-ingress.yaml`.

`cat example-ingress.yaml`{{execute}}

We will be using this file to create our Ingress resource.


2. Create the Ingress resource by running the following command:
    
    `kubectl apply -f example-ingress.yaml`{{execute}}

    Output:

    `ingress.networking.k8s.io/example-ingress created`

3. Verify the IP address is set:
    
    `kubectl get ingress`{{execute}}

    > **Note:** This can take a couple of minutes.

    ```
    NAME              HOSTS              ADDRESS       PORTS     AGE
    example-ingress   hello-world.info   172.17.0.15   80        38s
    ```

4. Add the following line to the bottom of the `/etc/hosts` file.

    > **Note:** If you are running Minikube locally, use minikube ip to get the external IP. The IP address displayed within the ingress list will be the internal IP.

    `172.17.0.15 hello-world.info`

    `echo "172.17.0.15 hello-world.info" >> /etc/hosts`

    This sends requests from hello-world.info to Minikube.

5. Verify that the Ingress controller is directing traffic:

    `curl hello-world.info`{{execute}}

    Output:

    ```
    Hello, world!
    Version: 1.0.0
    Hostname: web-55b8c6998d-8k564
    ```

    > **Note:** If you are running Minikube locally, you can visit hello-world.info from your browser.