# Test Your Ingress

1. Access the 1st version of the Hello World app.
`curl hello-world.info`{{execute}}

Output:
```
Hello, world!
Version: 1.0.0
Hostname: web-55b8c6998d-8k564
```

2. Access the 2nd version of the Hello World app.
`curl hello-world.info/v2`{{execute}}

Output:
```
Hello, world!
Version: 2.0.0
Hostname: web2-75cd47646f-t8cjk
```
