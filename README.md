# Containerizing Web App using Dockerfile
This guide explains how to containerize a web application using Dockerfile and deploy it on AWS EC2.
### Dockerfile
```dockerfile
FROM nginx:alpine
COPY src/ /usr/share/nginx/html
# ENV PRODUCTION=true
EXPOSE 80
# nginx defaults to this command
CMD ["nginx", "-g", "daemon off;"]
```
- FROM nginx:alpine:--------------------->Pulls the Nginx image based on Alpine Linux as the base image.
- COPY src/ /usr/share/nginx/html:-----> Copies the content of the local src/ directory to the Nginx HTML directory.
- ENV PRODUCTION=true:--------------->Optional line to set an environment variable (commented out in this example).
- EXPOSE 80:-------------------------------->Exposes port 80 to allow external access.
- CMD ["nginx", "-g", "daemon off;"]:-----> Sets the default command to start Nginx and run it in the foreground.
 
# Deploying on AWS EC2

### Launch EC2 Instance:

- Log in to your AWS Management Console.
- Navigate to EC2 and launch a new instance (Ubuntu or Amazon Linux).
- Ensure that port 80 is open in the security group of your EC2 instance.
- SSH into the Instance:

### Use SSH to connect to your EC2 instance:
```bash
ssh -i your-key.pem ec2-user@ec2-instance-public-ip
```
### Update the package index and install Docker:
```bash
sudo apt update
sudo apt install docker.io -y
```
- Copy your Dockerfile and web app files to the EC2 instance using SCP or any other method.
### Build Docker Image:
- Navigate to the directory containing your Dockerfile and build the Docker image:
```bash
docker build -t my-web-app .
```
- Run the Docker container based on the image you just built:
```bash
docker run -d -p 80:80 my-web-app
```
### Access the Web App:

- Open a web browser and enter your EC2 instance's public IP address. You should see your web app running.

### Optional: Production Setup:
### Install Docker Compose:
```bash
sudo apt install docker-compose -y
```
1. Building and Running Containers with Docker Compose:
- Create a docker-compose.yml file in your project directory with the following content:
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "80:80"
```
- Uncomment the ENV PRODUCTION=true line in your Dockerfile if you're deploying in a production environment.
- Make necessary adjustments to your Nginx configuration for production use.
### Cleanup:
- To stop and remove the Docker container when not in use:
```bash
docker stop container-id
docker rm container-id
```
or Docker-compose step
``` docker-compose up -d ```

This guide provides a basic setup for containerizing and deploying a web app using Docker and AWS EC2. Adjustments may be needed based on your specific application requirements and infrastructure setup.
