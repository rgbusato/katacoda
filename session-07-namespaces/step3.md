# Create pods in each namespace

A Kubernetes namespace provides the scope for Pods, Services, and Deployments in the cluster.

Users interacting with one namespace do not see the content in another namespace.

To demonstrate this, let's spin up a simple Deployment and Pods in the development namespace.

1. We first check what is the current context:

    `kubectl config view`{{execute}}

    Output:

    ```
    TODO
    ```

    `kubectl config current-context`{{execute}}

    Output:

    `TODO: lithe-cocoa-92103_kubernetes`

2. The next step is to define a context for the kubectl client to work in each namespace: 
   
    > **Note:** The value of `cluster` and `user` fields are copied from the current context.
    
    ```
    kubectl config set-context dev --namespace=development \
    --cluster=lithe-cocoa-92103_kubernetes \
    --user=lithe-cocoa-92103_kubernetes
    ```{{copy}}

    ```
    kubectl config set-context prod --namespace=production \
    --cluster=lithe-cocoa-92103_kubernetes \
    --user=lithe-cocoa-92103_kubernetes
    ```{{copy}}

    By default, the above commands adds two contexts that are saved into file `.kube/config`. You can now view the contexts and alternate against the two new request contexts depending on which namespace you wish to work against.


3. To view the new contexts:
    
    `kubectl config view`{{execute}}

    Output:

    ```
    TODO
    ```

4. Deploying to the `development` namespace:

    Let's switch to operate in the development namespace.

    `kubectl config use-context dev`{{execute}}

    You can verify your current context by doing the following:
    
    `kubectl config current-context`{{execute}}

    Output:

    `dev`

    *At this point, all requests we make to the Kubernetes cluster from the command line are scoped to the development namespace.*

    Let's create some contents:

    `cat snowflake-deployment.yaml`{{execute}}

    Output:
    
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
        labels:
            app: snowflake
        name: snowflake
    spec:
        replicas: 2
        selector:
            matchLabels:
                app: snowflake
        template:
            metadata:
                labels:
                    app: snowflake
            spec:
                containers:
                - image: k8s.gcr.io/serve_hostname
                  imagePullPolicy: Always
                  name: snowflake
    ```

    Apply the manifest to create a Deployment

    `kubectl apply -f snowflake-deployment.yaml`{{execute}}

    We have just created a deployment whose replica size is 2 that is running the pod called `snowflake` with a basic container that just serves the hostname.

    `kubectl get deployment`{{execute}}

    Output:
    
    ```
    NAME         READY   UP-TO-DATE   AVAILABLE   AGE
    snowflake    2/2     2            2           2m
    ```

    `kubectl get pods -l app=snowflake`{{execute}}

    Output:
    
    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    snowflake-3968820950-9dgr8   1/1       Running   0          2m
    snowflake-3968820950-vgc4n   1/1       Running   0          2m
    ```

    And this is great, developers are able to do what they want, and they do not have to worry about affecting content in the production namespace.

5. Deploying to the `production` namespace:

    Let's switch to the production namespace and show how resources in one namespace are hidden from the other.

    `kubectl config use-context prod`{{execute}}

    The `production` namespace should be empty, and the following commands should return nothing.
    
    `kubectl get deployment`{{execute}}
    
    `kubectl get pods`{{execute}}

    
    Production likes to run cattle, so let's create some *cattle pods*

    `kubectl create deployment cattle --image=k8s.gcr.io/serve_hostname`{{execute}}

    `kubectl scale deployment cattle --replicas=5`{{execute}}

    `kubectl get deployment`{{execute}}

    Output:

    ```
    NAME         READY   UP-TO-DATE   AVAILABLE   AGE
    cattle       5/5     5            5           10s
    ```

    `kubectl get pods -l app=cattle`{{execute}}

    Output:
    
    ```
    NAME                      READY     STATUS    RESTARTS   AGE
    cattle-2263376956-41xy6   1/1       Running   0          34s
    cattle-2263376956-kw466   1/1       Running   0          34s
    cattle-2263376956-n4v97   1/1       Running   0          34s
    cattle-2263376956-p5p3i   1/1       Running   0          34s
    cattle-2263376956-sxpth   1/1       Running   0          34s
    ```

    At this point, it should be clear that the resources users create in one namespace are hidden from the other namespace.

    As the policy support in Kubernetes evolves, we will extend this scenario to show how you can provide different authorization rules for each namespace.
