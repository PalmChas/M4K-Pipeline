# Kubernetes Deploy

This folder contains Kubernetes manifests for the app:
- `Deployment` (`first-pipeline`)
- `Service` (`ClusterIP`)
- `Ingress` (`m4k-pipeline.local`)
- `HPA` (CPU autoscaling)

## 1) Build and push image
Use any container registry. Update image in `k8s/deployment.yaml`:
`ghcr.io/palmchas/m4k-pipeline:latest`

## 2) Deploy with kustomize
```bash
kubectl apply -k k8s
kubectl get all -n m4k-pipeline
```

## 3) Test locally with port-forward
```bash
kubectl port-forward svc/first-pipeline 8080:80 -n m4k-pipeline
curl http://localhost:8080/status
curl http://localhost:8080/health
curl http://localhost:8080/metrics
curl http://localhost:8080/metrics/prometheus
```

## Notes
- `Ingress` requires an ingress controller (for example `ingress-nginx`).
- `HPA` requires metrics server in the cluster.
