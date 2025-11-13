# 🚀 Flask Kubernetes CI/CD Pipeline

[![CI Pipeline](https://img.shields.io/badge/CI-GitHub%20Actions-2088FF?logo=github-actions&logoColor=white)](https://github.com)
[![CD Pipeline](https://img.shields.io/badge/CD-Jenkins-D24939?logo=jenkins&logoColor=white)](https://jenkins.io)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io)
[![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)](https://docker.com)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://python.org)

> A production-ready CI/CD pipeline demonstrating modern DevOps practices with Kubernetes orchestration, automated testing, and continuous delivery.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Kubernetes Features](#-kubernetes-features-used)
- [Prerequisites](#-prerequisites)
- [Local Development](#-local-development-with-docker)
- [Kubernetes Deployment](#-kubernetes-deployment)
- [Jenkins Pipeline](#-jenkins-pipeline-deployment)
- [CI/CD Workflow](#-cicd-workflow)
- [Scaling & Load Balancing](#-scaling--load-balancing)
- [Rollout Strategies](#-automated-rollouts)
- [Testing](#-testing)
- [Troubleshooting](#-troubleshooting)

---

## 🎯 Overview

This project implements a complete **Continuous Integration and Continuous Delivery (CI/CD)** pipeline for a Python Flask application using industry-standard DevOps tools. The application is containerized with Docker, orchestrated with Kubernetes, and deployed through an automated Jenkins pipeline.

### What This Project Demonstrates

✅ **Version Control Best Practices** - Branch protection, pull request workflows, and code reviews  
✅ **Automated Testing** - Linting with flake8 and unit testing with pytest  
✅ **Containerization** - Multi-stage Docker builds for optimized images  
✅ **Kubernetes Orchestration** - Deployments, Services, scaling, and rolling updates  
✅ **CI with GitHub Actions** - Automated testing and Docker image building  
✅ **CD with Jenkins** - Automated deployment to Kubernetes clusters  
✅ **Production-Ready Features** - Health checks, resource limits, and load balancing

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Developer                                │
│                            ↓                                     │
│                    Git Push to Branch                            │
└─────────────────────────────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│                     GitHub Actions (CI)                          │
│  • Run flake8 linting (max-line-length: 90)                     │
│  • Execute pytest unit tests                                     │
│  • Build Docker image                                            │
└─────────────────────────────────────────────────────────────────┘
                             ↓
                    Pull Request Review
                             ↓
                    Merge to main branch
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│                     Jenkins Pipeline (CD)                        │
│  Stage 1: Build Docker Image                                    │
│  Stage 2: Deploy to Kubernetes                                  │
│  Stage 3: Verify Deployment                                     │
└─────────────────────────────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│                  Kubernetes Cluster (Minikube)                   │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │   Pod 1      │  │   Pod 2      │  │   Pod 3      │         │
│  │ Flask App    │  │ Flask App    │  │ Flask App    │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│         ↑                  ↑                  ↑                  │
│         └──────────────────┴──────────────────┘                 │
│                            │                                     │
│                   ┌────────────────┐                            │
│                   │ Load Balancer  │                            │
│                   │  (Service)     │                            │
│                   └────────────────┘                            │
│                            ↓                                     │
│                      NodePort :30007                             │
└─────────────────────────────────────────────────────────────────┘
                             ↓
                          Users
```

---

## ⚙️ Kubernetes Features Used

### 1. Deployments

- Declarative application management
- Maintains desired state of 3 replica pods
- Enables version control of application releases
- Automatic pod recreation on failure

### 2. Rolling Update Strategy

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1 # Max 1 extra pod during update
    maxUnavailable: 1 # Max 1 pod can be unavailable
```

- **Zero-downtime deployments** - Application stays available during updates
- **Gradual rollout** - New version deployed incrementally
- **Automatic rollback** - Revert to previous version if issues detected

### 3. Horizontal Scaling

- **3 replica pods** running simultaneously
- Easy scaling with `kubectl scale` command
- Distributes load across multiple instances
- High availability and fault tolerance

### 4. Load Balancing

- **NodePort Service** exposes application on port 30007
- Automatically distributes traffic across all healthy pods
- Built-in health checking and traffic routing
- Session affinity support

### 5. Resource Management

```yaml
resources:
  requests:
    memory: "128Mi"
    cpu: "250m"
  limits:
    memory: "256Mi"
    cpu: "500m"
```

- **Resource requests** - Guaranteed minimum resources
- **Resource limits** - Prevents resource exhaustion
- Efficient cluster resource utilization
- Quality of Service (QoS) guarantees

### 6. Self-Healing

- Automatic pod restart on failure
- Health check integration
- Maintains desired replica count
- Automatic rescheduling on node failure

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

| Tool         | Version | Purpose                  |
| ------------ | ------- | ------------------------ |
| **Docker**   | 20.10+  | Container runtime        |
| **Python**   | 3.11+   | Application runtime      |
| **Minikube** | 1.30+   | Local Kubernetes cluster |
| **kubectl**  | 1.27+   | Kubernetes CLI           |
| **Jenkins**  | 2.400+  | CI/CD automation         |
| **Git**      | 2.30+   | Version control          |

### Installation Quick Links

- [Docker Installation](https://docs.docker.com/get-docker/)
- [Minikube Installation](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl Installation](https://kubernetes.io/docs/tasks/tools/)
- [Jenkins Installation](https://www.jenkins.io/doc/book/installing/)

---

## 🐳 Local Development with Docker

### Step 1: Clone the Repository

```bash
git clone https://github.com/your-username/flask-k8s-ci-cd-assignment.git
cd flask-k8s-ci-cd-assignment
```

### Step 2: Build the Docker Image

```bash
docker build -t flask-k8s-ci-cd-assignment:latest .
```

**What happens during the build:**

- Uses multi-stage build for optimized image size
- Stage 1: Installs Python dependencies
- Stage 2: Creates minimal runtime image
- Final image size: ~150MB (vs ~1GB without multi-stage)

### Step 3: Run the Container Locally

```bash
docker run -d -p 5000:5000 --name flask-app flask-k8s-ci-cd-assignment:latest
```

### Step 4: Test the Application

```bash
# Using curl
curl http://localhost:5000

# Or open in browser
# Navigate to: http://localhost:5000
```

**Expected Output:**

```
Hello World, pod id <container-hostname>!
```

### Step 5: View Container Logs

```bash
docker logs flask-app
```

### Step 6: Stop and Remove Container

```bash
docker stop flask-app
docker rm flask-app
```

---

## ☸️ Kubernetes Deployment

### Step 1: Start Minikube Cluster

```bash
# Start minikube
minikube start --driver=docker

# Verify cluster is running
minikube status
```

**Expected Output:**

```
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### Step 2: Configure Docker Environment

```bash
# Point Docker CLI to minikube's Docker daemon
eval $(minikube docker-env)

# Build image in minikube's Docker
docker build -t flask-k8s-ci-cd-assignment:latest .
```

> **Why this matters:** This ensures Kubernetes can access the image without pushing to a registry.

### Step 3: Deploy to Kubernetes

```bash
# Apply all Kubernetes manifests
kubectl apply -f kubernetes/

# Or apply individually
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
```

### Step 4: Verify Deployment

```bash
# Check deployment status
kubectl get deployments

# Check pods
kubectl get pods

# Check services
kubectl get services

# Detailed deployment info
kubectl describe deployment flask-deployment
```

**Expected Output:**

```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
flask-deployment   3/3     3            3           30s

NAME                                READY   STATUS    RESTARTS   AGE
flask-deployment-7d8f9c5b6d-abc12   1/1     Running   0          30s
flask-deployment-7d8f9c5b6d-def34   1/1     Running   0          30s
flask-deployment-7d8f9c5b6d-ghi56   1/1     Running   0          30s

NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
flask-service   NodePort   10.96.123.456   <none>        5000:30007/TCP   30s
```

### Step 5: Access the Application

```bash
# Get minikube IP
minikube ip

# Access application
curl http://$(minikube ip):30007

# Or open in browser
minikube service flask-service
```

### Step 6: Monitor Deployment

```bash
# Watch pods in real-time
kubectl get pods -w

# View pod logs
kubectl logs -f <pod-name>

# View all pod logs
kubectl logs -l app=flask-app
```

---

## 🔧 Jenkins Pipeline Deployment

### Prerequisites Setup

#### 1. Install Jenkins Plugins

Navigate to **Manage Jenkins → Manage Plugins** and install:

- Kubernetes CLI Plugin
- Docker Pipeline Plugin
- Git Plugin
- Pipeline Plugin

#### 2. Configure kubectl in Jenkins

```bash
# On Jenkins server, copy kubeconfig
mkdir -p /var/lib/jenkins/.kube
cp ~/.kube/config /var/lib/jenkins/.kube/config
chown -R jenkins:jenkins /var/lib/jenkins/.kube

# Verify Jenkins can access kubectl
sudo -u jenkins kubectl get nodes
```

#### 3. Configure Docker Access

```bash
# Add Jenkins user to docker group
sudo usermod -aG docker jenkins

# Restart Jenkins
sudo systemctl restart jenkins
```

### Creating the Jenkins Pipeline

#### Step 1: Create New Pipeline Job

1. Open Jenkins dashboard
2. Click **New Item**
3. Enter name: `flask-k8s-deployment`
4. Select **Pipeline**
5. Click **OK**

#### Step 2: Configure Pipeline

1. **General Section:**

   - ✅ GitHub project
   - Project URL: `https://github.com/your-username/flask-k8s-ci-cd-assignment`

2. **Build Triggers:**

   - ✅ GitHub hook trigger for GITScm polling
   - ✅ Poll SCM: `H/5 * * * *` (checks every 5 minutes)

3. **Pipeline Section:**
   - Definition: **Pipeline script from SCM**
   - SCM: **Git**
   - Repository URL: `https://github.com/your-username/flask-k8s-ci-cd-assignment.git`
   - Branch: `*/main`
   - Script Path: `Jenkinsfile`

#### Step 3: Configure GitHub Webhook (Optional)

1. Go to GitHub repository → **Settings** → **Webhooks**
2. Click **Add webhook**
3. Payload URL: `http://<jenkins-url>/github-webhook/`
4. Content type: `application/json`
5. Events: **Just the push event**
6. ✅ Active

### Running the Pipeline

#### Manual Trigger

```bash
# From Jenkins UI
1. Navigate to the pipeline job
2. Click "Build Now"
3. Monitor console output
```

#### Automatic Trigger

```bash
# Push to main branch
git push origin main

# Jenkins automatically detects and builds
```

### Pipeline Stages Explained

#### Stage 1: Build Docker Image

```groovy
stage('Build Docker Image') {
    steps {
        sh 'docker build -t flask-k8s-ci-cd-assignment:latest .'
    }
}
```

- Builds Docker image from Dockerfile
- Tags image as `latest`
- Uses Docker cache for faster builds

#### Stage 2: Deploy to Kubernetes

```groovy
stage('Deploy to Kubernetes') {
    steps {
        sh 'kubectl apply -f kubernetes/deployment.yaml'
        sh 'kubectl apply -f kubernetes/service.yaml'
    }
}
```

- Applies Kubernetes manifests
- Creates/updates deployment and service
- Triggers rolling update if changes detected

#### Stage 3: Verify Deployment

```groovy
stage('Verify Deployment') {
    steps {
        sh 'kubectl rollout status deployment/flask-deployment'
        sh 'kubectl get pods'
        sh 'kubectl get services'
    }
}
```

- Waits for rollout to complete
- Verifies all pods are running
- Displays deployment status

### Monitoring Pipeline Execution

```bash
# View Jenkins console output
# Shows real-time build progress

# Check Kubernetes rollout status
kubectl rollout status deployment/flask-deployment

# View deployment history
kubectl rollout history deployment/flask-deployment
```

---

## 🔄 CI/CD Workflow

### Complete Development Workflow

```
1. Developer creates feature branch
   ↓
2. Developer commits code changes
   ↓
3. Developer pushes to GitHub
   ↓
4. GitHub Actions CI triggers automatically
   ├─ Runs flake8 linting
   ├─ Executes pytest tests
   └─ Builds Docker image
   ↓
5. Developer creates Pull Request to develop
   ↓
6. Admin reviews code and CI results
   ↓
7. Admin approves and merges to develop
   ↓
8. Developer creates PR from develop to main
   ↓
9. Admin merges to main branch
   ↓
10. Jenkins CD pipeline triggers automatically
    ├─ Stage 1: Build Docker Image
    ├─ Stage 2: Deploy to Kubernetes
    └─ Stage 3: Verify Deployment
    ↓
11. Application deployed to Kubernetes cluster
    ↓
12. Rolling update ensures zero downtime
```

### GitHub Actions CI Pipeline

The `.github/workflows/ci.yml` workflow runs on every push:

```yaml
✓ Checkout code
✓ Set up Python 3.11
✓ Install dependencies
✓ Run flake8 (max-line-length: 90)
✓ Run pytest unit tests
✓ Build Docker image
```

**Viewing CI Results:**

- Navigate to **Actions** tab in GitHub
- Click on workflow run
- View detailed logs for each step

---

## 📈 Scaling & Load Balancing

### Horizontal Pod Autoscaling

#### Manual Scaling

```bash
# Scale up to 5 replicas
kubectl scale deployment flask-deployment --replicas=5

# Verify scaling
kubectl get pods

# Scale down to 2 replicas
kubectl scale deployment flask-deployment --replicas=2
```

#### Automatic Scaling (HPA)

```bash
# Create Horizontal Pod Autoscaler
kubectl autoscale deployment flask-deployment \
  --cpu-percent=50 \
  --min=3 \
  --max=10

# View HPA status
kubectl get hpa

# Describe HPA
kubectl describe hpa flask-deployment
```

**How it works:**

- Monitors CPU utilization across pods
- Scales up when average CPU > 50%
- Scales down when CPU < 50%
- Maintains between 3-10 replicas

### Load Balancing in Action

#### Test Load Distribution

```bash
# Make multiple requests
for i in {1..10}; do
  curl http://$(minikube ip):30007
  echo ""
done
```

**Expected Output:**

```
Hello World, pod id flask-deployment-7d8f9c5b6d-abc12!
Hello World, pod id flask-deployment-7d8f9c5b6d-def34!
Hello World, pod id flask-deployment-7d8f9c5b6d-ghi56!
Hello World, pod id flask-deployment-7d8f9c5b6d-abc12!
...
```

Notice different pod IDs - traffic is distributed across all pods!

#### Load Balancing Features

✅ **Round-robin distribution** - Even traffic spread  
✅ **Health checking** - Only routes to healthy pods  
✅ **Session affinity** - Can enable sticky sessions  
✅ **Automatic failover** - Removes unhealthy pods

### Monitoring Load

```bash
# Watch pod resource usage
kubectl top pods

# View service endpoints
kubectl get endpoints flask-service

# Describe service
kubectl describe service flask-service
```

---

## 🔄 Automated Rollouts

### Rolling Update Strategy

Our deployment uses a **RollingUpdate** strategy for zero-downtime deployments:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1 # Max 1 extra pod during update
    maxUnavailable: 1 # Max 1 pod unavailable during update
```

### How Rolling Updates Work

```
Initial State: 3 pods running (v1)
[Pod-1] [Pod-2] [Pod-3]

Step 1: Create 1 new pod (v2) - maxSurge allows this
[Pod-1] [Pod-2] [Pod-3] [Pod-4-v2]

Step 2: Terminate 1 old pod - maxUnavailable allows this
[Pod-1] [Pod-2] [Pod-4-v2]

Step 3: Create another new pod (v2)
[Pod-1] [Pod-2] [Pod-4-v2] [Pod-5-v2]

Step 4: Terminate another old pod
[Pod-1] [Pod-4-v2] [Pod-5-v2]

Step 5: Create final new pod (v2)
[Pod-1] [Pod-4-v2] [Pod-5-v2] [Pod-6-v2]

Step 6: Terminate last old pod
[Pod-4-v2] [Pod-5-v2] [Pod-6-v2]

Final State: 3 pods running (v2)
```

### Performing Updates

#### Update Application Code

```bash
# 1. Make code changes
vim app.py

# 2. Build new Docker image
eval $(minikube docker-env)
docker build -t flask-k8s-ci-cd-assignment:v2 .

# 3. Update deployment image
kubectl set image deployment/flask-deployment \
  flask-container=flask-k8s-ci-cd-assignment:v2

# 4. Watch rollout progress
kubectl rollout status deployment/flask-deployment
```

#### Monitor Rollout

```bash
# Watch pods being replaced
kubectl get pods -w

# View rollout status
kubectl rollout status deployment/flask-deployment

# Check rollout history
kubectl rollout history deployment/flask-deployment
```

### Rollback Capabilities

#### Automatic Rollback

Kubernetes automatically rolls back if:

- New pods fail to start
- Readiness probes fail
- Image pull fails

#### Manual Rollback

```bash
# Rollback to previous version
kubectl rollout undo deployment/flask-deployment

# Rollback to specific revision
kubectl rollout undo deployment/flask-deployment --to-revision=2

# View rollout history
kubectl rollout history deployment/flask-deployment

# View specific revision details
kubectl rollout history deployment/flask-deployment --revision=3
```

### Pause and Resume Rollouts

```bash
# Pause rollout (for canary testing)
kubectl rollout pause deployment/flask-deployment

# Make additional changes
kubectl set resources deployment/flask-deployment -c=flask-container --limits=cpu=600m

# Resume rollout
kubectl rollout resume deployment/flask-deployment
```

### Benefits of Rolling Updates

✅ **Zero Downtime** - Application stays available during updates  
✅ **Gradual Rollout** - Issues detected before full deployment  
✅ **Easy Rollback** - Quick revert to previous version  
✅ **Resource Efficient** - Minimal extra resources needed  
✅ **Automated** - Kubernetes handles the complexity

---

## 🧪 Testing

### Local Testing

#### Run Unit Tests

```bash
# Install dependencies
pip install -r requirements.txt

# Run pytest
pytest test_helper.py -v

# Run with coverage
pytest --cov=. test_helper.py
```

#### Run Linting

```bash
# Run flake8
flake8 . --max-line-length=90 --exclude=venv,__pycache__

# Auto-fix with autopep8
autopep8 --in-place --aggressive --aggressive *.py
```

### Kubernetes Testing

#### Test Deployment

```bash
# Verify all pods are running
kubectl get pods -l app=flask-app

# Test pod connectivity
kubectl exec -it <pod-name> -- curl localhost:5000

# Test service connectivity
kubectl run test-pod --image=curlimages/curl --rm -it --restart=Never \
  -- curl flask-service:5000
```

#### Test Scaling

```bash
# Scale to 5 replicas
kubectl scale deployment flask-deployment --replicas=5

# Verify all pods are ready
kubectl wait --for=condition=ready pod -l app=flask-app --timeout=60s

# Test load distribution
for i in {1..20}; do
  curl http://$(minikube ip):30007
done
```

#### Test Rolling Updates

```bash
# Update image
kubectl set image deployment/flask-deployment \
  flask-container=flask-k8s-ci-cd-assignment:v2

# Verify zero downtime
while true; do
  curl -s http://$(minikube ip):30007 || echo "FAILED"
  sleep 0.5
done
```

#### Test Rollback

```bash
# Introduce a bad deployment
kubectl set image deployment/flask-deployment \
  flask-container=flask-k8s-ci-cd-assignment:broken

# Watch it fail
kubectl get pods -w

# Rollback
kubectl rollout undo deployment/flask-deployment

# Verify recovery
kubectl rollout status deployment/flask-deployment
```

---

## 🔍 Troubleshooting

### Common Issues and Solutions

#### Issue: Pods Not Starting

```bash
# Check pod status
kubectl get pods

# View pod details
kubectl describe pod <pod-name>

# Check pod logs
kubectl logs <pod-name>

# Common causes:
# - Image pull errors (check imagePullPolicy)
# - Resource constraints (check node resources)
# - Configuration errors (check deployment.yaml)
```

#### Issue: Service Not Accessible

```bash
# Verify service exists
kubectl get service flask-service

# Check service endpoints
kubectl get endpoints flask-service

# Verify pod labels match service selector
kubectl get pods --show-labels

# Test from within cluster
kubectl run test --image=curlimages/curl --rm -it --restart=Never \
  -- curl flask-service:5000
```

#### Issue: Image Not Found

```bash
# Ensure using minikube's Docker daemon
eval $(minikube docker-env)

# Rebuild image
docker build -t flask-k8s-ci-cd-assignment:latest .

# Verify image exists
docker images | grep flask-k8s

# Check imagePullPolicy in deployment.yaml
# Should be: imagePullPolicy: Never (for local images)
```

#### Issue: Jenkins Can't Access Kubernetes

```bash
# Verify kubectl works for Jenkins user
sudo -u jenkins kubectl get nodes

# Check kubeconfig permissions
ls -la /var/lib/jenkins/.kube/config

# Copy correct kubeconfig
sudo cp ~/.kube/config /var/lib/jenkins/.kube/config
sudo chown jenkins:jenkins /var/lib/jenkins/.kube/config
```

#### Issue: Rolling Update Stuck

```bash
# Check rollout status
kubectl rollout status deployment/flask-deployment

# View rollout history
kubectl rollout history deployment/flask-deployment

# Check for pod errors
kubectl describe pods -l app=flask-app

# Force rollback if needed
kubectl rollout undo deployment/flask-deployment
```

### Useful Debug Commands

```bash
# Get all resources
kubectl get all

# Describe deployment
kubectl describe deployment flask-deployment

# View events
kubectl get events --sort-by='.lastTimestamp'

# Check resource usage
kubectl top nodes
kubectl top pods

# Access pod shell
kubectl exec -it <pod-name> -- /bin/sh

# Port forward for debugging
kubectl port-forward <pod-name> 5000:5000
```

---

## 📚 Additional Resources

### Documentation

- [Kubernetes Official Docs](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Jenkins Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [GitHub Actions Docs](https://docs.github.com/en/actions)

### Useful Commands Cheat Sheet

```bash
# Kubernetes
kubectl get pods                          # List pods
kubectl get deployments                   # List deployments
kubectl get services                      # List services
kubectl logs <pod-name>                   # View logs
kubectl describe pod <pod-name>           # Pod details
kubectl exec -it <pod-name> -- /bin/sh   # Access pod shell
kubectl delete pod <pod-name>             # Delete pod
kubectl scale deployment <name> --replicas=5  # Scale

# Docker
docker build -t <image-name> .           # Build image
docker run -p 5000:5000 <image-name>     # Run container
docker ps                                 # List containers
docker logs <container-id>                # View logs
docker exec -it <container-id> /bin/sh   # Access shell
docker stop <container-id>                # Stop container

# Minikube
minikube start                            # Start cluster
minikube stop                             # Stop cluster
minikube status                           # Check status
minikube ip                               # Get cluster IP
minikube service <service-name>           # Open service
minikube dashboard                        # Open dashboard
eval $(minikube docker-env)               # Use minikube Docker
```

---

## 👥 Contributors

- **Member A (Admin)** - Repository administration, branch protection, minikube setup, PR reviews
- **Member B (Developer)** - Application code, Docker assets, Kubernetes manifests, CI/CD configuration

---

## 📄 License

This project is created for educational purposes as part of a DevOps assignment.

---

## 🎓 Learning Outcomes

By completing this project, you will have hands-on experience with:

✅ Git workflows and branch protection strategies  
✅ Automated testing and continuous integration  
✅ Container orchestration with Kubernetes  
✅ Infrastructure as Code (IaC) practices  
✅ CI/CD pipeline design and implementation  
✅ Production deployment strategies  
✅ Monitoring and troubleshooting distributed systems

---

<div align="center">

**Built with ❤️ using Flask, Docker, Kubernetes, and Jenkins**

⭐ Star this repo if you found it helpful!

</div>
