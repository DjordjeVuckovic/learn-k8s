# Useful commands

## kubectl
```bash
kubectl get all
```

```bash
kubectl get nodes
```

```bash
kubectl get svc -n=yournamespace
```

```bash
kubectl get deployments -o wide
```

```bash
kubectl get pods -o wide
```

```bash
kubectl logs podname
```

```bash
kubectl describe pod podname
```

```bash
kubectl apply -f .
```

## minikube
```bash
minikube start
```

```bash
minikube dashboard
```
-  Known issue - Minikube IP not accessible so run the following command to get the IP and tunnel to it
```bash
minikube service service-name
```

```bash
minikube stop
```