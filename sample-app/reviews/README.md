BookInfo Application - Reviews
Review is a Java Microservice whch contains book reviews. It calls the ratings microservice.
Build a docker image:
```
docker build -t details .
```
Push the image to DockerHub or ECR and replace the IMAGE variable in k8s/deployment.yaml with the Docker Image.
Deploy service using command:
```
kubectl apply -f k8s
