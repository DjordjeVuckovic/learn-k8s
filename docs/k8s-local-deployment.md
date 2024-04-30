# K8s local deployment

- You need to have already installed docker on you local machine
- Install kubectl (see docs https://kubernetes.io/docs/tasks/tools/)
- I used command below
```Run
choco install kubernetes-cli
```
- Install minikube (see docs https://kubernetes.io/docs/tasks/tools/)
- I used command below
```Run
choco install minikube
```
- After that run minikube
```bash
minikube start
minikube dashboard
```
- You can use minikube tunnel to expose the service to the external world
```bash
minikube tunnel
```

- Also, you need to install helm (see docs https://helm.sh/docs/intro/install/)
- Helm is a package manager for k8s
- I used command below
```bash
choco install kubernetes-helm
```
