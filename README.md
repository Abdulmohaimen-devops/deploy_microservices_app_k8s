# deploy_microservices_app_k8s

Option 3: Using Pre-Built Container Images

    💡 Recommended if you want to deploy the app faster in fewer steps to an existing cluster.

NOTE: If you need to create a Kubernetes cluster locally or on the cloud, follow "Option 1" or "Option 2" until you reach the skaffold run step.

This option offers you pre-built public container images that are easy to deploy by deploying the release manifest directly to an existing cluster.

Prerequisite: a running Kubernetes cluster (either local or on cloud).

    Clone this repository, and go to the repository directory

    Run kubectl apply -f ./release/kubernetes-manifests.yaml to deploy the app.

    Run kubectl get pods to see pods are in a Ready state.

    Find the IP address of your application, then visit the application on your browser to confirm installation.

    kubectl get service/frontend-external



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

## Example app

### Example app (from istio)

export PATH="$PATH:~/istio-1.0.3/bin"
kubectl apply -f <(istioctl kube-inject -f app.yml)
```

