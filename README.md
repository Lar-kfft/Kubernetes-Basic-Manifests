# Kubernetes Basic Manifests

Базовые Kubernetes манифесты для деплоя приложений (Deployment с 3 репликами Nginx, Service типа LoadBalancer, ConfigMap с конфигурацией Nginx)

## nginx-deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```
## nginx-service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```
## nginx-configmap.yaml
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    server {
        listen 80;
        server_name localhost;
        
        location / {
            root /usr/share/nginx/html;
            index index.html;
        }
    }
```
## Использование
```bash
# Deploy all resources
kubectl apply -f .

# Check deployment
kubectl get deployments
kubectl get pods
kubectl get services

# Delete resources
kubectl delete -f .

# Применить конкретный файл
kubectl apply -f nginx-deployment.yaml

# Применить несколько конкретных файлов
kubectl apply -f nginx-deployment.yaml -f nginx-service.yaml

# Применить все файлы в папке
kubectl apply -f manifests/

# Применить по шаблону
kubectl apply -f *.yaml
```

