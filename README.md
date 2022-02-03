# k8s-demo

1) Create simple hello world project using Spring Boot
2) Run build image plugin (!!! We should always specify image version)
3) Create 'backend' directory
4) Create deployment|service|ingress.yaml files in 'backend' directory
5) In order to use Ingress we need to install nginx-ingress using helm:
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
6) Verify that ingress-nginx is installed:
kubectl --namespace ingress-nginx get services -o wide -w ingress-nginx-controller
kubectl get service ingress-nginx-controller --namespace=ingress-nginx
kubectl get pods --namespace=ingress-nginx
7) Verify all services:
kubectl get svc --all-namespaces
8) Deploy backend pod:
kubectl apply -f deployment.yaml
kubectl get pods
9) Deploy backend service:
kubectl apply -f service.yaml
kubectl get svc
10) Deploy backend-ingress:
kubectl apply -f ingress.yaml
kubectl get ingress
11) Endpoint should be available on http://localhost/hello
12) Update spring-boot app to use env variable and build image with version 0.0.2
13) Create Config-Map for backend pod:
kubectl create configmap backend-config-v1 --from-literal=defaultName=k8sCluster
kubectl get configmap
14) Update all resources in the cluster for "backend":
kubectl apply -f .
15) Verify that env variable is taken from k8s config-map:
http://localhost/api/hello/with-name
16) Create 'redis' folder and add deployment|service|service-headless.yaml files
17) Update spring-boot app to use redis - add new endpoints
18) Create k8s secret for redis password: kubectl create secret generic redis-passwd --from-literal=passwd=${RANDOM}
(Секреты в Kubernetes по умолчанию хранятся в незашифрованном виде!!!)
19) Deploy redis with Cluster IP and check that app can call it using IP: 10.110.40.246
20) Deploy redis without Cluster IP and check that app can call it using DNS name: redis-0.redis
(It can be useful if we need to write to master node but read from any node of db)
21) Deploy application as helm charts
22) In any dir execute helm create backend
23) Copy all k8s files into templates dir
24) In deployment.yaml file add {{ .Values.replicaCount }} placeholder in front of spec.replicas
25) Create env dir and create two dir: dev|prod and add values.yaml with different values for placeholder replicaCount
26) Deploy our service as helm chart: helm install RELEASE_NAME ./backend --values ./backend/env/prod/values.yaml
