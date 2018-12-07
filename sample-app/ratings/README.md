BookInfo Application - Ratings
Ratings is a Javascript microservice which fetches ratings from a MySQL Database. We have provisioned a MySQL RDS DB Instance and imported the dump provided in "mysql" subdirectory.
The RDS credentials can be replaced in k8s/deployment.yaml. If using Jenkinsfile, you can securely store your RDS details with Jenkins Credentials:
* Secret text "rds-host" for Database host and 
* Encrypted Username/Password "rds-creds" for Database Username/Password.
Build a docker image:
```
docker build -t details .
```
Push the image to DockerHub or ECR and replace the IMAGE variable in k8s/deployment.yaml with the Docker Image.
Deploy service using command:
```
kubectl apply -f k8s
```
