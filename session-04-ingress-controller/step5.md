# Edit Ingress

1. A new path was added to our ingress.
    ```
        - path: /v2/*
          backend:
            serviceName: web2
            servicePort: 8080
    ```
    Take a look at the full file:
    `cat example-ingress-2.yaml`{{execute}}

2. Apply the changes:

    `kubectl apply -f example-ingress-2.yaml`{{execute}}

    Output:

    `ingress.extensions/example-ingress configured`
