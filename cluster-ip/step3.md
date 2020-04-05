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

You should get something like:
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
       ...
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
...
```