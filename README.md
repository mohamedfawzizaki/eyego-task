App Url : http://af8f8f7f3d0654858a2e8c776621d63e-449083115.us-east-1.elb.amazonaws.com/

# Eyego Task

A simple project that deploys a Node.js app returning "Hello Eyego" to an AWS EKS cluster. 
The setup is modular for easy migration to GCP (GKE) or Alibaba Cloud (ACK).

## Steps

1.Setup AWS EKS Cluster
   - Used AWS Management Console to create an EKS cluster for simplicity.

2.Create Node.js App

3.Dockerize the App
   
4.Created an ECR repository named `eyego-app`.
   - Built and pushed the image:
     docker build -t eyego-app .
     docker tag eyego-app:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/eyego-app:latest
     docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/eyego-app:latest

5.Create Kubernetes Manifests
   - eyego-deployment.yaml: 
        - Defines deployment with 2 replicas.
   - eyego-service.yaml: 
        - Exposes the app using a LoadBalancer service.

6.Deploy to EKS
   - Applied manifests to the cluster:
        kubectl apply -f k8s/eyego-deployment.yaml
        kubectl apply -f k8s/eyego-service.yaml
   - Retrieved the external IP:
        kubectl get svc eyego-service

7.Set Up CI/CD with GitHub Actions
   - Created .github/workflows/ci-cd.yml to:
        - Build the Docker image.
        - Push to AWS ECR.
        - Deploy to AWS EKS automatically on pushes to main.

8.Push Project to GitHub
   - Initialized a GitHub repo.
   - Pushed the entire project.

---

## Migration Notes

The project is designed for easy migration to:
- GCP:  
  - Push images to Google Container Registry or Artifact Registry.
  - Update kubectl context using "gcloud container clusters get-credentials".
- Alibaba Cloud:  
  - Push images to Alibaba Container Registry.
  - Update kubeconfig using aliyun cs "DescribeClusterUserKubeconfig".

Kubernetes manifests remain the same; only the container registry and kubeconfig commands change.

---

## Access the App

access the app using the EXTERNAL-IP of "eyego-service":

App Url : http://af8f8f7f3d0654858a2e8c776621d63e-449083115.us-east-1.elb.amazonaws.com/




Note : 
i skip some security best practices just for simplicity, as the this is just a demo task.
one of these best practices is to use AWS Organizations with IAM Identity Center to issue short-term credentials tied to a user directory, it is ot applied here as this demo uses standard long-term credentials for simplicity.  