# 🚀 Senior Cheatsheet: .NET + Docker + Kubernetes

---

# 🟦 .NET CLI (Advanced)

## Create Solutions & Microservices
```bash
dotnet new sln -n MySolution

dotnet new webapi -n ServiceA

dotnet new webapi -n ServiceB

dotnet sln add ServiceA/ServiceA.csproj

dotnet sln add ServiceB/ServiceB.csproj
```

## Multi-project build
```bash
dotnet build MySolution.sln
```

## Publish optimized (container-ready)
```bash
dotnet publish -c Release -o out /p:UseAppHost=false
```

## Run with environment
```bash
ASPNETCORE_ENVIRONMENT=Production dotnet run
```

---

# 🐳 Docker (Production-grade)

## Multi-stage build (standard)
```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:10.0
WORKDIR /app

ENV ASPNETCORE_URLS=http://+:8080

COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "Service.dll"]
```

## Build image
```bash
docker build -t myservice:latest .
```

## Run with port mapping
```bash
docker run -p 8080:8080 myservice
```

## Inspect runtime
```bash
docker exec -it container_id sh
```

## Logs streaming
```bash
docker logs -f container_id
```

## Image inspection
```bash
docker inspect image_id
```

---

# 🧩 Docker Compose (Microservices orchestration)

## Full stack run
```bash
docker compose up -d
```

## Build + recreate
```bash
docker compose up --build --force-recreate
```

## Scale service
```bash
docker compose up --scale servicea=3
```

## Health check logs
```bash
docker compose logs -f
```

---

# ☸️ Kubernetes (Production orchestration)

## Cluster overview
```bash
kubectl cluster-info
kubectl get nodes -o wide
```

## Workloads
```bash
kubectl get pods -A
kubectl get deployments
kubectl get replicasets
```

## Deep inspection
```bash
kubectl describe pod pod_name
kubectl describe svc service_name
```

## Logs (critical)
```bash
kubectl logs -f pod_name
kubectl logs deployment/deployment_name
```

## Exec into container
```bash
kubectl exec -it pod_name -- sh
```

## Apply manifests
```bash
kubectl apply -f k8s/
```

## Rollout management
```bash
kubectl rollout status deployment/name
kubectl rollout restart deployment/name
kubectl rollout history deployment/name
```

## Debug networking
```bash
kubectl get svc
kubectl get endpoints
kubectl get ingress
```

---

# 🌐 Services & Networking Model

## Core concept
```
Ingress -> Service -> Pod -> Container
```

## Check service routing
```bash
kubectl describe svc service_name
```

Expected:
```
Port: 80
TargetPort: 8080
Endpoints: pod_ip:8080
```

---

# ⚡ Ingress (NGINX)

## Get ingress
```bash
kubectl get ingress
```

## Describe ingress
```bash
kubectl describe ingress name
```

## Check controller
```bash
kubectl get pods -n ingress-nginx
```

---

# 🧠 Observability & Debugging

## Event stream
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

## Resource usage
```bash
kubectl top pod
kubectl top node
```

## Full diagnosis flow
```bash
kubectl get pods
kubectl describe pod
kubectl logs pod
kubectl get svc
kubectl get endpoints
```

---

# 🚀 CI/CD Flow (Senior pattern)

```bash
dotnet test

dotnet publish -c Release

docker build -t registry/app:tag .

docker push registry/app:tag

kubectl apply -f k8s/
```

---

# 🧱 Architecture Pattern

## Microservices stack
```
API Gateway (Ingress / YARP / Nginx)
        ↓
Services (Pods)
        ↓
Databases / Message Brokers
```

---

# 🔥 Best Practices

- Always use `IHttpClientFactory` in .NET
- Never hardcode service IPs in Kubernetes
- Use readiness/liveness probes
- Prefer ClusterIP for internal services
- Use Ingress instead of NodePort in production
- Use multi-stage Docker builds

---

# 🧾 Senior Debug Checklist

1. Pod running?
2. Service selector correct?
3. Endpoints exist?
4. TargetPort correct?
5. App listening on 0.0.0.0?
6. Logs clean?

---

# 🚀 End of Senior Cheatsheet

