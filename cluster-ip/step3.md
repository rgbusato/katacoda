Here is how we can internally reach to our service.

1. Let's spin up an interactive Pod that we can use to test requests to reach our Service:
    
    `kubectl run my-shell --rm -i --tty --image ubuntu -- bash`{{execute}}

2. Once the bash prompt within the pod is available, first we must install curl:

    ```
    apt-get update
    apt-get install curl
    ```{{execute}}

3. Within the pod, we can make a request to our previously created ClusterIP service. 
    
    Like so:
    `curl http://my-nginx`{{execute}}

    You should get a html output saying `Welcome to nginx!`