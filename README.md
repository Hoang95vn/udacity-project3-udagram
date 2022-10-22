Udagram app URL: https://a036a331e91da4d2097ffa4ee11e6635-328085331.us-east-1.elb.amazonaws.com/

docker-compose config
# Run from the directory where you have the compose file present
docker-compose down
# To delete all dangling images
docker image prune --all

docker-compose -f docker-compose-build.yaml build --parallel

docker-compose up

aG9hbmdudGg=
THVhYmFvbjc4OTUxMjM=

# Apply env variables and secrets
kubectl apply -f aws-secret.yaml
kubectl apply -f env-secret.yaml
kubectl apply -f env-configmap.yaml
# Deployments - Double check the Dockerhub image name and version in the deployment files
kubectl apply -f backend-feed-deployment.yaml
kubectl apply -f backend-user-deployment.yaml
kubectl apply -f frontend-deployment.yaml
kubectl apply -f reverseproxy-deployment.yaml
# Do the same for other three deployment files
# Service
kubectl apply -f backend-feed-service.yaml
kubectl apply -f backend-user-service.yaml
kubectl apply -f frontend-service.yaml
kubectl apply -f reverseproxy-service.yaml
# Do the same for other three service files

# Check the deployment names and their pod status
kubectl get deployments
# Create a Service object that exposes the frontend deployment
# The command below will ceates an external load balancer and assigns a fixed, external IP to the Service.
kubectl expose deployment reverseproxy --type=LoadBalancer --name=publicreverseproxy
kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontend
# Repeat the process for the *reverseproxy* deployment. 
# Check name, ClusterIP, and External IP of all deployments
kubectl get services 
kubectl get pods # It should show the STATUS as Running

# Run these commands from the /udagram-frontend directory
docker build . -t kaiservn/udagram-frontend:v5
docker push kaiservn/udagram-frontend:v5

# Run these commands from the /udagram-deployment directory
# Rolling update the containers of "frontend" deployment
kubectl set image deployment frontend frontend=kaiservn/udagram-frontend:v5