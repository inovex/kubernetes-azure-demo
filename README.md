# kubernetes-azure-demo
Small demo how to deploy an application to on premise and azure. 


## How to run demo

The demo has some prerequisites. You have to have make, minikube, kubectl and the azure cli v2 installed. Also you have to have an active azure subscription. You can run this demo with a [trial subscription](https://azure.microsoft.com/en-us/free/).

The demo is tested with the following versions:
```
 az  --version
azure-cli (2.0.0)

acs (2.0.0)
appservice (0.1.1b5)
batch (0.1.1b4)
cloud (2.0.0)
component (2.0.0)
configure (2.0.0)
container (0.1.1b4)
core (2.0.0)
documentdb (0.1.1b2)
feedback (2.0.0)
iot (0.1.1b3)
keyvault (0.1.1b5)
network (2.0.0)
nspkg (2.0.0)
profile (2.0.0)
redis (0.1.1b3)
resource (2.0.0)
role (2.0.0)
sql (0.1.1b5)
storage (2.0.1)
vm (2.0.0)

Python (Linux) 3.6.0 (default, Jan 16 2017, 12:12:55) 
[GCC 6.3.1 20170109]

1 christoph@petrausch-thinkpad ..ex-gitlab/Brownbags/microsoft-roadshow (git)-[master] % minikube version                                                                                                                                 :(
minikube version: v0.16.0
christoph@petrausch-thinkpad ..ex-gitlab/Brownbags/microsoft-roadshow (git)-[master] % kubectl version
Client Version: version.Info{Major:"1", Minor:"5", GitVersion:"v1.5.3", GitCommit:"029c3a408176b55c30846f0faedf56aae5992e9b", GitTreeState:"clean", BuildDate:"2017-02-15T06:40:50Z", GoVersion:"go1.7.4", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"5", GitVersion:"v1.5.2", GitCommit:"08e099554f3c31f6e6f07b448ab3ed78d0520507", GitTreeState:"clean", BuildDate:"2017-01-12T04:52:34Z", GoVersion:"go1.7.4", Compiler:"gc", Platform:"linux/amd64"}

```

To setup the demo, login to azure first:
```
az login
```

After the login you can create the azure environment. Run 
```
cd demo
make all
```
This will create the complete azure environment.
The command will print out the credentials of the created redis cache. You have to change the login credentials in the `paas/k8s-deployment/todo-app-config.yaml` file.

To setup a clean minikube environment run:
```
minikube delete && minikube start
```

Now we have setup two environments. The minikube environment is our on premise environment and the azure container service the cloud environment. 


### Deploy into On Premise Environment

The following commands deploy the ToDo Application and a redis master-slave setup into the minikube. 
```
kubectl config use-context minikube
kubectl apply -f iaas/k8s-deployment/
```

The following command will give you an URL to access the application in your browser.
```
minikube service --namespace=todo-app todo-app --url
```


### Deploy into Azure Environment
The following commands deploy the ToDo Application into the [ACS](https://azure.microsoft.com/en-us/services/container-service/) Kubernetes.
```
kubectl config use-context minikube
kubectl apply -f paas/k8s-deployment/
```

Wait until the ToDo service get a public IP:
```
kubectl get svc --namespace=todo-app
```
Now you can use that public IP to access the service in your browser.

# Tear Down
To tear down the demo, delete the ressource group `kubernetes-demo` in your azure subscription.