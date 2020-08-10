# Create the User credentials

> **Note:** **Kubernetes does not have API Objects for User Accounts**. 

Of the available ways to manage authentication (see [Kubernetes official documentation](https://kubernetes.io/docs/admin/authentication) for a complete list), we will use OpenSSL certificates for their simplicity.

1. Create a private key for your user. 
   
   In this example, we will name the file `employee.key`

   `openssl genrsa -out employee.key 2048`{{execute}}

    Output:

    ```bash
    Generating RSA private key, 2048 bit long modulus (2 primes)
    ......+++++
    .......+++++
    e is 65537 (0x010001)
    ```

2. Create a certificate sign request `employee.csr`:
    
    Create a certificate sign request `employee.csr` using the private key you just created (`employee.key` in this example). 
    
    Make sure you specify your username and group in the `-subj` section (CN is for the username and O for the group). 
    
    We will use `employee` as the **name** and `slalom` as the **group**:

    `openssl req -new -key employee.key -out employee.csr -subj "/CN=employee/O=slalom"`{{execute}}

    > **Note:** Don't worry if you see the following error in the output:

    ```bash
    Can't load /root/.rnd into RNG
    139912319340992:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:88:Filename=/root/.rnd
    ```

3. Locate your Kubernetes cluster certificate authority (CA)
    
    This will be responsible for approving the request and generating the necessary certificate to access the cluster API

    In the case of Minikube, it would be `~/.minikube/`. 
    
    Check that the files `ca.crt` and `ca.key` exist in the location:

    `ls -ltr ~/.minikube/`{{execute}}

    Set the env variable `CA_LOCATION`:

    `export CA_LOCATION=~/.minikube`{{execute}}

4. Generate the final certificate `employee.crt`

    Generate the final certificate `employee.crt` by approving the certificate sign request, `employee.csr`, you made earlier. 
    
    In this example, the certificate will be valid for 500 days:

    ```
    openssl x509 -req -in employee.csr -CA $CA_LOCATION/ca.crt -CAkey $CA_LOCATION/ca.key -CAcreateserial -out employee.crt -days 500
    ```{{execute}}

    Output:

    ```
    Signature ok
    subject=CN = employee, O = slalom
    Getting CA Private Key
    ```

5. Save both `employee.crt` and `employee.key` in a safe location 
   
    In this example we will use `/home/employee/.certs/`:

    `mkdir -p /home/employee/.certs/`{{execute}}

    `cp ./employee.* /home/employee/.certs/`{{execute}}

    ```bash
    $ ls -ltr /home/employee/.certs/
    total 12
    -rw------- 1 root root 1675 Aug  9 20:27 employee.key
    -rw-r--r-- 1 root root  911 Aug  9 20:27 employee.csr
    -rw-r--r-- 1 root root 1013 Aug  9 20:27 employee.crt
    ```

6. Add a new context with the new credentials for your Kubernetes cluster
    
    This example is for a Minikube cluster but it should be similar for others:

    ```
    kubectl config set-credentials employee --client-certificate=/home/employee/.certs/employee.crt  --client-key=/home/employee/.certs/employee.key
    ```{{execute}}

    Output:

    `User "employee" set.`

    ```
    kubectl config set-context employee-context --cluster=minikube --namespace=office --user=employee
    ```{{execute}}

    Output:

    `Context "employee-context" created.`

7. Verify your current access

    Now you should get an **access denied error** when using the kubectl CLI with this configuration file. 
    
    > **Note:** This is expected as we have not defined any permitted operations for this user

    `kubectl --context=employee-context get pods`{{execute}}

    Output:

    ```
    Error from server (Forbidden): pods is forbidden: User "employee" cannot list resource "pods" in API group "" in the namespace "office"
    ```
