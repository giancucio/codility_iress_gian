# codility_iress_gian

Build a “Hello World” container with built-in observability
Using any IaC tooling, deploy an internet facing “hello world” web container on AWS.

It can be any existing image and does not have to be built from scratch. This can be done on
any container platform (K8S, ECS, EC2).

--------------------------------------------------------------------------------------------

Prerequisites
1. setup terraform
2. setup kubectl
3. setup awscli
4. setup ```aws configure```

--------------------------------------------------------------------------------------------

1. Create Cluster
```
terraform init
```
```
terraform apply
```
2. Configure kubectl and Deploy Kubernetes Dashboad with Metrics Server for Observability
```
aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)
```
```
wget -O v0.3.6.tar.gz https://codeload.github.com/kubernetes-sigs/metrics-server/tar.gz/v0.3.6 && tar -xzf v0.3.6.tar.gz
```
```
kubectl apply -f metrics-server-0.3.6/deploy/1.8+/
```
```
kubectl get deployment metrics-server -n kube-system
```
```
kubectl apply -f recommended.yaml
```
```
kubectl proxy
```
3. Authenticate Kubernetes Dashboard (Execute on a different terminal) (Get token and apply in the Dashboard)
```
kubectl apply -f kubernetes-dashboard-admin.rbac.yaml
```
```
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep service-controller-token | awk '{print $1}')
```
4. Deploy Helloworld Image
```
kubectl apply -f helloworld.yaml
```
```
kubectl apply -f helloworld-service.yaml
```
5. Get Details of Running Helloworld Container
```
kubectl get svc service-helloworld -o yaml
```

------------------------------------------------------------------------------------------
Hello World
https://user-images.githubusercontent.com/107382465/173346399-7bdb5ba4-86ab-45a0-8df7-2bd1b7372a3a.JPG

Dashboard
https://user-images.githubusercontent.com/107382465/173346406-62b2cbc6-e489-4a93-99a5-be0016dd5d04.JPG
