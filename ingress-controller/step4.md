# Create Second Deployment

1. Create a v2 Deployment using the following command:

    `kubectl run web2 --image=gcr.io/google-samples/hello-app:2.0 --port=8080`{{execute}}

    Output:

    `deployment.apps/web2 created`

2. Expose the Deployment:

    `kubectl expose deployment web2 --target-port=8080 --type=NodePort`{{execute}}

    Output:
    
    `service/web2 exposed`
