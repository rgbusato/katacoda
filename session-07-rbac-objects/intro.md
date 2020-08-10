# In this example you will learn the following:

* verify RBAC is enabled for our cluster
* create a Namespace
* create a User Account using OpenSSL certificates
* create a Role for managing deployments
* Bind the Role to the employee user
* test the RBAC rule (User Account)
* create a ServiceAccount (not default) within our new Namespace
* deploy a pod within that namespace that uses the ServiceAccount we've created
* test the same RBAC rule (Service Account)
