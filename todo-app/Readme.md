# Todo Web App

## Running the application

### Namespace

```bash
$ kubectl create -f todo-app/web-app-namespace.yml
$ kubectl get namespaces
```

## Deploy the app

```Bash
# Start the Redis Master Replication Controller
kubectl create -f todo-app/redis-master-controller.yml
# Start the Redis Master service
kubectl create -f todo-app/redis-master-service.yml
# Check if the Redis Master is already created
kubectl get pods
# Start the Redis Slave Replication Controller
kubectl create -f todo-app/redis-slave-controller.yml
# Start the Redis Slave service
kubectl create -f todo-app/redis-slave-service.yml
# Check if the Redis Slave is already created
kubectl get pods
# Start the Redis Slave Replication Controller
kubectl create -f todo-app/todo-app-controller.yml
# Start the Redis Slave service
kubectl create -f todo-app/todo-app-service.yml
# Check if the Todo App is already created
kubectl get pods -l name=todo-app-web
```

### TLDR

```Bash
kubectl create -f todo-app/
```

## Validate everything

```bash
kubectl get svc
kubectl get rc
kubectl get endpoints
```

## Logging

Show logs from the Redis Master

```bash
kubectl logs -f {pod id}
```

## Autoscaling

```bash
kubectl autoscale --min=1 --cpu-percent=80 --max=5 todo-app/todo-app-controller.yml
```

## Rolling Update

Make a rolling update without downtime

```bash
kubectl rolling-update todo-app-web --image=johscheuer/todo-app-web:v2 --update-period="30s"
```

# Todo

- [ ] Use deployments
- [ ] Update Rolling update
