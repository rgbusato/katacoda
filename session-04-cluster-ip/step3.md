Here is how we can internally reach to our service within the Cluster.

1. Let's spin up an interactive Pod that we can use to test requests to reach our Service:
    
    `kubectl run curl --rm --image=radial/busyboxplus:curl -i --tty`{{execute}}

2. Within the curl pod, we can make a request to our previously created ClusterIP service. 
    
    Like so:
    
    `curl http://hello-app:8080`{{execute}}

    Output:
    
    ```
    Hello, world!
    Version: 1.0.0
    Hostname: hello-app-5f85f64f6c-5zc6p
    ```
