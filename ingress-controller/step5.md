# Edit Ingress

1. Edit the existing `example-ingress.yaml` and add the following lines:
```
    - path: /v2/*
        backend:
          serviceName: web2
          servicePort: 8080
```

2. Apply the changes:
`kubectl apply -f example-ingress.yaml`{{execute}}

Output:
`ingress.extensions/example-ingress configured`
