## BookInfo Application - ProductPage
ProductPage is a Python microservice which calls the details and reviews to populate the page. It is the frontend of the application.
Build a docker image:
```
docker build -t productpage .
```
Push the image to DockerHub or ECR and replace the IMAGE variable in k8s/deployment.yaml with the Docker Image.
Deploy it using command:
```
kubectl apply -f k8s
```
