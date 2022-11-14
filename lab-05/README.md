# Kubernetes Workshop
Lab 05: Deploying applications using Ingress

---

## Instructions

### Install the Ingress Controller (nginx)

 - Create a new directory to store the required files
```
mkdir lab-05
cd lab-05
```

 - Create a new namespace for the ingress controller
```
kubectl create namespace ingress-nginx
```

 - Deploy the required resources of the Ingress controller:
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.1/deploy/static/provider/cloud/deploy.yaml
```

 - Verify NGINX controller installation 
```
kubectl get pods -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx --watch
```

 - Inspect the ingress controller Service
```
kubectl get svc ingress-nginx-controller -n ingress-nginx
```

### Deploy the application

 - Inspect the "**Cats**" application (pod and ClusterIP service)
```
curl https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/cats.yaml
```

 - Deploy the "**Cats**" application
```
kubectl create -f https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/cats.yaml
```

 - Inspect the "**Dogs**" application (pod and ClusterIP service)
```
curl https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/dogs.yaml
```

 - Deploy the "**Dogs**" application

```
kubectl create -f https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/dogs.yaml
```

 - Inspect the "**Birds**" application (pod and ClusterIP service)
```
curl https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/birds.yaml
```

 - Deploy the "**Birds**" application
```
kubectl create -f https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/birds.yaml
```

 - Inspect the ingress resource
```
curl https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/ingress.yaml
```

 - Create the ingress resource
```
kubectl create -f https://gitlab.com/sela-kubernetes-workshop/lab-05/raw/master/ingress.yaml
```

 - List existing pods
```
kubectl get pods
```

 - List existing services
```
kubectl get svc
```

 - List existing ingress 
```
kubectl get ingress
```

### Access the application

 - Get the ingress controller External IP 
```
kubectl get svc ingress-nginx-controller -n ingress-nginx
```

 - Browse to the cats service
```
http://<ingress-controller-external-ip>/cats
```

 - Browse to the dogs service
```
http://<ingress-controller-external-ip>/dogs
```

 - Browse to the birds service
```
http://<ingress-controller-external-ip>/birds
```

### Cleanup

 - List existing resources
```
kubectl get all
```

 - Delete ingress resource
```
kubectl delete ingress --all
```

 - Delete existing resources
```
kubectl delete all --all
kubectl delete all --all -n ingress-nginx
kubectl delete namespace ingress-nginx
```

 - List existing resources
```
kubectl get all
```
