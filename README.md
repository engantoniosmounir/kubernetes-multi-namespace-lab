# Lab 3: Multi-Tenancy with Namespaces and Internal Routing

## Scenario
Your team is adopting a "cluster per environment" strategy, but for cost reasons, you must use a single cluster for Development (dev) and Staging (staging) environments. You need to ensure logical separation and enable communication between two applications within the same namespace.

## Lab Description
The student will create two namespaces (dev and staging). They will deploy a two-tier application (a frontend and a backend) in the dev namespace and a separate instance in the staging namespace. They will use services for internal pod networking and demonstrate environment isolation.

---
## Build Instructions for Students

The students will need to build these Docker images before deploying to Kubernetes. build commands:


# Build backend image
```bash
docker build -f Dockerfile.backend -t backend-app:latest .
```

# Build frontend image
```
docker build -f Dockerfile.frontend -t frontend-app:latest .
```

# If using minikube, load the images
```
minikube image load backend-app:latest
minikube image load frontend-app:latest
```

--- 

## Tasks to Implement

1. Create two namespaces: `dev` and `staging`.

2. In the `dev` namespace, deploy a backend pod (a simple Python HTTP server or redis) and expose it via a ClusterIP service named `backend-service`.

3. In the `dev` namespace, deploy a frontend pod (nginx). Configure it so that it can communicate with the backend pod using the service name `backend-service`.

4. Repeat steps 2 and 3 in the `staging` namespace, ensuring the frontend there communicates with its own local `backend-service`.

---

## Notes for Students

- The backend server runs on port `8080` and provides three endpoints: `/`, `/health`, and `/info`

- The frontend nginx is configured to proxy requests to the backend using the service name `backend-service`

- The HTML interface provides buttons to test connectivity to different backend endpoints

- Both applications display their pod name and namespace for easy identification when testing environment isolation

--- 
> [!IMPORTANT]
> Required Deliverables
* Push your Puplic GitHub Repo to LMS
# The repository should be structured as follows:
1. dev-environment.yaml
2. staging-environment.yaml
3. README.md
## The README.md file must include screenshots of the output from the following commands:

- Ensure that the status of pods and services in dev and staging namespace are running
```bash
kubectl get pods,svc -n dev
```
```bash
kubectl get pods,svc -n staging
```
- Execute port-forward to can access the app from the browser
```bash 
kubectl port-forward pod/frontend-pod 8082:80 -n dev
```
- open your browser at localhost:8082 then take the screenshot
