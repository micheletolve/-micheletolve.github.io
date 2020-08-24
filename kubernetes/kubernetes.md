# Kubernetes

Procedure per creare un cluster kubernetes su azure

Dopo aver creato un AKS cluster dal portale azure

```
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

```
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

```
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

```
helm install nginx-ingress stable/nginx-ingress --namespace default
```

create pod, creare service

```
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80

```

apiVersion - Which version of the Kubernetes API you’re using to create this object
kind - What kind of object you want to create
metadata - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
spec - What state you desire for the object
