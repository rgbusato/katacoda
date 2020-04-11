# Enable the Ingress controller

1. To enable the NGINX Ingress controller, run the following command:

    `minikube addons enable ingress`{{execute}}

2. Verify that the NGINX Ingress controller is running

    `kubectl get pods -n kube-system`{{execute}}

    > **Note:** This can take up to a minute.

    Output:

    ```
    NAME                                        READY   STATUS    RESTARTS   AGE
    coredns-6955765f44-4phmx                    1/1     Running   0          37s
    coredns-6955765f44-j8z5z                    1/1     Running   0          37s
    etcd-minikube                               1/1     Running   0          39s
    kube-apiserver-minikube                     1/1     Running   0          39s
    kube-controller-manager-minikube            1/1     Running   0          39s
    kube-proxy-sqj72                            1/1     Running   0          37s
    kube-scheduler-minikube                     1/1     Running   0          39s
    nginx-ingress-controller-6fc5bcc8c9-4n8z6   0/1     Running   0          37s
    storage-provisioner                         1/1     Running   0          41s
    ```