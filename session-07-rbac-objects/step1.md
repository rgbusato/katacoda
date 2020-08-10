ref= https://docs.bitnami.com/tutorials/configure-rbac-in-your-kubernetes-cluster/#use-case-2-enable-helm-in-your-cluster
# Create Namespace

1. Create Namespace

  `kubectl create namespace office`{{execute}}

  Output:

  `namespace/office created`


# Create the user credentials

`openssl genrsa -out employee.key 2048`{{execute}}

Output:

```
Generating RSA private key, 2048 bit long modulus (2 primes)
......+++++
.......+++++
e is 65537 (0x010001)
```

`openssl req -new -key employee.key -out employee.csr -subj "/CN=employee/O=slalom"`{{execute}}

> **Note:** You may see the following error in the output:

```
Can't load /root/.rnd into RNG
139912319340992:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:88:Filename=/root/.rnd
```

```
export CA_LOCATION=/etc/kubernetes/pki
openssl x509 -req -in employee.csr -CA $CA_LOCATION/ca.crt -CAkey $CA_LOCATION/ca.key -CAcreateserial -out employee.crt -days 500
```{{execute}}

Output:
```
Signature ok
subject=CN = employee, O = slalom
Getting CA Private Key
```

`mkdir -p /home/employee/.certs/`{{execute}}
`cp ./employee.* /home/employee/.certs/`{{execute}}

```
controlplane $ ls -ltr /home/employee/.certs/
total 12
-rw------- 1 root root 1675 Aug  9 20:27 employee.key
-rw-r--r-- 1 root root  911 Aug  9 20:27 employee.csr
-rw-r--r-- 1 root root 1013 Aug  9 20:27 employee.crt
```

```
kubectl config set-credentials employee --client-certificate=/home/employee/.certs/employee.crt  --client-key=/home/employee/.certs/employee.key
```{{execute}}

Output:

`User "employee" set.`

```
kubectl config set-context employee-context --cluster=kubernetes --namespace=office --user=employee
```{{execute}}

Output:

`Context "employee-context" created.`

`kubectl --context=employee-context get pods`{{execute}}

Output:

```
Error from server (Forbidden): pods is forbidden: User "employee" cannot list resource "pods" in API group "" in the namespace "office"
```

# Step 3

`cat role-deployment-manager.yaml`{{execute}}

```
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  namespace: office
  name: deployment-manager
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["deployments", "replicasets", "pods"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"] # You can also use ["*"]
```

`kubectl create -f role-deployment-manager.yaml`{{execute}}

Output:

`role.rbac.authorization.k8s.io/deployment-manager created`

List the Role created (optional):
```
controlplane $ kubectl get roles -n office
NAME                 CREATED AT
deployment-manager   2020-08-09T20:35:29Z
```

# Step 4 Bind the role to the employee user

`cat rolebinding-deployment-manager.yaml`{{execute}}

```
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: deployment-manager-binding
  namespace: office
subjects:
- kind: User
  name: employee
  apiGroup: ""
roleRef:
  kind: Role
  name: deployment-manager
  apiGroup: ""
```

`kubectl create -f rolebinding-deployment-manager.yaml`{{execute}}

Output:

`rolebinding.rbac.authorization.k8s.io/deployment-manager-binding created`


# Step 5 - Test RBAC rule for the User
```
kubectl --context=employee-context run --image bitnami/dokuwiki mydokuwiki
kubectl --context=employee-context get pods
```


The following command will fail:

`kubectl --context=employee-context get pods --namespace=default`{{execute}}

Output:

```
Error from server (Forbidden): pods is forbidden: User "employee" cannot list resource "pods" in API group "" in the namespace "default"
```

**Now you have created a user with limited permissions in your cluster.**


-------

# test the pod permissions - end of this SO tread: https://stackoverflow.com/questions/42642170/how-to-run-kubectl-commands-inside-a-container


```
kubectl --context=employee-context run --image bitnami/dokuwiki mydokuwiki

kubectl exec -it <your-container-with-the-attached-privs> -- /kubectl get pods -n <YOUR_NAMESPACE>
kubectl run curl --rm --image=radial/busyboxplus:curl -i --tty 
```

# Create a POD (will use the default serviceaccount) 

`kubectl run --namespace office --image bitnami/dokuwiki mydokuwiki`

`kubectl get pods -n office`

# Create a new POD (will use the specific ServiceAccount we've created)

`cat robot-pod.yaml`{{execute}}
```
apiVersion: v1
kind: Pod
metadata:
  name: robot
  namespace: office
spec:
  serviceAccountName: robot
  containers:
  - name: kubectl
    image: bitnami/kubectl:latest
    command: ['/bin/bash', '-c', 'echo "Hello, Kubernetes!" && sleep 3600']
```

`kubectl create -f robot-pod.yaml`{{execute}}

Output:

```
controlplane $ kubectl get pods -n office
NAME         READY   STATUS    RESTARTS   AGE
robot        1/1     Running   0          9s
mydokuwiki   1/1     Running   0          6m27s
```

Verify the pod is running with the service account we've created for it:
`kubectl get pod/robot -n office -o yaml`{{execute}}

----
```
...
# service account info
serviceAccount: robot
serviceAccountName: robot
...
# robot serviceaccount token mounted to the pod
volumes:
  - name: robot-token-4brqx
    secret:
      defaultMode: 420
      secretName: robot-token-4brqx
```
