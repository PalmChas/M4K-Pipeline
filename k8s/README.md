# Kubernetes Deploy

This folder contains Kubernetes manifests for the app:
- `Deployment` (`first-pipeline`, backend)
- `Service` (`first-pipeline`, backend `ClusterIP`)
- `ConfigMap` (`frontend-nginx-config`)
- `Deployment` (`frontend`, nginx reverse proxy)
- `Service` (`frontend`, `ClusterIP`)
- `Ingress` (`m4k-pipeline.local` -> `frontend`)
- `HPA` (`first-pipeline`, CPU autoscaling)

## 1) Build image for local cluster
The backend deployment uses `first-pipeline:k8s-v4` in `k8s/deployment.yaml`.
Build it before deploy:
```bash
docker build -t first-pipeline:k8s-v4 .
```
If you use kind:
```bash
kind load docker-image first-pipeline:k8s-v4 --name m4k
```

## 2) Deploy with kustomize
```bash
kubectl apply -k k8s
kubectl get all -n m4k-pipeline
```

## 3) Test locally with port-forward
```bash
kubectl port-forward svc/frontend 8080:80 -n m4k-pipeline
curl http://localhost:8080/status
curl http://localhost:8080/health
curl http://localhost:8080/metrics
curl http://localhost:8080/metrics/prometheus
curl http://localhost:8080/k8s
```

## Notes
- Bronze proof: `kubectl get deploy,svc -n m4k-pipeline` should show at least 2 Deployments and 2 Services.
- `Ingress` requires an ingress controller (for example `ingress-nginx`).
- `HPA` requires metrics server in the cluster.
