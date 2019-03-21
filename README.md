# deploy_microservices_app_k8s
this deployment use this repo https://github.com/GoogleCloudPlatform/microservices-demo.git
clone this repo and move router.yml and deployment.yml to cloned repo
I will use Option 3: Using Pre-Built Container Images

    💡 Recommended if you want to deploy the app faster in fewer steps to an existing cluster.

NOTE: If you need to create a Kubernetes cluster locally or on the cloud, follow "Option 1" or "Option 2" until you reach the skaffold run step.

This option offers you pre-built public container images that are easy to deploy by deploying the release manifest directly to an existing cluster.

Prerequisite: a running Kubernetes cluster (either local or on cloud).

Clone this repository, and go to the repository directory


```
kubectl apply -f ./release/kubernetes-manifests.yaml  #to deploy the app.
```

Find the IP address of your application, then visit the application on your browser to confirm installation.

```
kubectl get service/frontend-external
minikube service frontend-external --url
```


# istio


## download (1.0.3):
```
cd ~
wget https://github.com/istio/istio/releases/download/1.0.3/istio-1.0.3-osx.tar.gz # macos
tar -xzvf istio-1.0.3-linux.tar.gz
cd istio-1.0.3
echo 'export PATH="$PATH:~/istio-1.0.3/bin"' >> ~/.profile
```


## Istio install

Apply CRDs:

```
kubectl apply -f ~/istio-1.0.3/install/kubernetes/helm/istio/templates/crds.yaml
```

Wait a few seconds.


Option 1: with no mutual TLS authentication (the next demos assume no mutual TLS)
```
kubectl apply -f ~/istio-1.0.3/install/kubernetes/istio-demo.yaml
```


## Disable LoadBalancer

Replace LoadBalancer in NodePort with:

```
kubectl edit -n istio-system svc/istio-ingressgateway
```

Get the URL with:
```
minikube service istio-ingressgateway -n istio-system --url
```


### Example app (from istio)

```
export PATH="$PATH:~/istio-1.0.3/bin"
kubectl apply -f <(istioctl kube-inject -f release/kubernetes-manifests.yaml) #	inject istio container
kubectl apply -f release/istio-manifests.yaml  #add istio components
```


now istio is enables 



### use istio as advanced descion maker 

Use Istio to make 10% of http request go to your custom fork of the frontend

```
kubectl apply -f deployement2.yml  # add another app to forward some of traffic 
kubectl apply -f router.yml   # add router decsions and vService
```


now the traffic divided into two pods by 


### Integrate Istio with OpenTracing using a tool like Jaeger


use this command from https://istio.io/docs/tasks/telemetry/distributed-tracing/jaeger/

```
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686  &
``` 

open in your laptop http://localhost:16686 and you will track all trafic

