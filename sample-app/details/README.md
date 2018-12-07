BookInfo Application - Details

Details is a Ruby microservice which contains book information.
Build a docker image:
```
docker build -t details .
```
Push the image to DockerHub or ECR and replace the IMAGE variable in k8s/deployment.yaml with the Docker Image.
Deploy it using command:
```
kubectl apply -f k8s
