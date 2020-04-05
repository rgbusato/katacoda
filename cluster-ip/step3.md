Here is how we can internally reach to our service.

Let's spin up an interactive Pod that we can use to test requests to reach our Service:
`kubectl run my-shell --rm -i --tty --image ubuntu -- bash`{{execute}}

Within the pod, we can make a request to our previously created ClusterIP service. Like so:
`curl http://my-nginx`{{execute}}

You should get a similar response back:
`TODO: add expected response here`