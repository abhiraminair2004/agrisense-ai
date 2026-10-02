# Deployment guide

## Docker Compose

```bash
docker compose build
docker compose up -d
docker compose ps
```

## Kubernetes

```bash
kubectl apply -f k8s/client-deployment.yaml
kubectl apply -f k8s/client-service.yaml
kubectl apply -f k8s/server-deployment.yaml
kubectl apply -f k8s/server-service.yaml
kubectl apply -f k8s/model-service-deployment.yaml
kubectl apply -f k8s/model-service-service.yaml
```

## Rolling update

Use immutable tags for deployment demonstrations, such as `v1` and `v2`, instead of the `latest` tag.

```bash
kubectl apply -f k8s/client-deployment.yaml
kubectl apply -f k8s/server-deployment.yaml
kubectl apply -f k8s/model-service-deployment.yaml
kubectl rollout status deployment/client
kubectl rollout status deployment/server
kubectl rollout status deployment/model-service
kubectl rollout history deployment/server
kubectl get pods
```

## Rollback

```bash
kubectl rollout history deployment/server
kubectl rollout undo deployment/server
kubectl rollout status deployment/server
kubectl get pods
```

The same rollback pattern applies to the other deployments when needed.

## Local verification

```bash
curl http://localhost:5000/health
curl http://localhost:8000/health
curl http://localhost:3000
```

A browser upload through the UI is validated by posting a file to the API gateway, which forwards to the model service and returns a prediction or a 422 validation error for unrelated images.
