
# <p style="color:teal">Kuberenetes two nodes cluster using minikube on mac</p>

Setting up multi nodes kuberenetes cluster using minikube

## <p style="color:teal">Authors</p>

- [@sharantechuser](https://www.github.com/sharantechuser)


## <p style="color:teal">Install minikube & kubectl</p>
In prerequisites, install Homebrew

#### 1. Install minikube via Homebrew

```sh
    # Install minikube
    brew install minikube

    # Verify minikube
    minikube version
```

#### 2. Install kubectl via Homebrew

```sh
    # Install kubectl
    brew install kubectl

    # Verify kubectl
    kubectl version
```


## <p style="color:teal">Setup two nodes kubernetes cluster 

#### 1. Start minikube cluster
1. This command creates and starts a Minikube VM or container with a Kubernetes cluster.
It creates 2 nodes one is master node and another one is worker node

```sh
    minikube start --nodes 2 -p local-k8s-cluster --driver=docker
```

#### 2. Verify the nodes
```sh
    kubectl get nodes
```
System shows two nodes *local-k8s-cluster* and *local-k8s-cluster-m02*  in Ready state.

#### 3. Verify cluster status 
```sh
    minikube status -p local-k8s-cluster
```

## <p style="color:teal">App Deployments in kubernetes cluster</p>

#### 1. create deployment manifest i.e nginx-deployment.yaml
```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: my-deployment
    spec:
    replicas: 3
    selector:
        matchLabels:
        app: my-app
    template:
        metadata:
        labels:
            app: my-app
        spec:
        containers:
            - name: my-container
            image: nginx
            ports:
                - containerPort: 8080

```

#### 2. create deployment
```sh
    kubectl apply -f nginx.yaml
```

#### 3. create service manifest i.e nginx-svc.yaml
```
    apiVersion: v1
    kind: Service
    metadata:
    name: my-app-service
    spec:
    type: NodePort
    selector:
        app: my-app
    ports:
        - protocol: TCP
        port: 80
        targetPort: 8080
        nodePort: 30007

```

#### 4. create service
```sh
    kubectl apply -f nginx-svc.yaml
```

#### 5. Verify the deployments and services
```sh
    kubectl get pods
    kubectl get svc
```

## <p style="color:teal">Kubernetes dashboard</p>

```sh
    # This create proxy URL for kubernetes dashboard
    minikube dashboard --url -p local-k8s-cluster
```

Open the URL from above response

## <p style="color:teal">Test the application</p>
<p>Test with below CURL command.</p>

```http
    curl http://localhost:30007
```