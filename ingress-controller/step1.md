# Enable the Ingress controller

1. To enable the NGINX Ingress controller, run the following command:

    `minikube addons enable ingress`{{execute}}

2. Verify that the NGINX Ingress controller is running

    `kubectl get pods -n kube-system`{{execute}}

    > **Note:** This can take up to a minute.

    Output:
    
    ```
    NAME                                        READY     STATUS    RESTARTS   AGE
    default-http-backend-59868b7dd6-xb8tq       1/1       Running   0          1m
    kube-addon-manager-minikube                 1/1       Running   0          3m
    kube-dns-6dcb57bcc8-n4xd4                   3/3       Running   0          2m
    kubernetes-dashboard-5498ccf677-b8p5h       1/1       Running   0          2m
    nginx-ingress-controller-5984b97644-rnkrg   1/1       Running   0          1m
    storage-provisioner                         1/1       Running   0          2m
    ```