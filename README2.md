How to Host Multiple Dockerized Applications Inside One VM Listening to Different Ports

In today’s era of containerization, Docker has emerged as a popular choice for deploying and managing applications. When hosting multiple Dockerized applications within a single virtual machine (VM), it becomes necessary to ensure each application can listen to a different port for communication. This blog will guide you through the process of hosting multiple Dockerized applications on a single VM, each listening to its own unique port.

Prerequisites: Before diving into the steps, ensure that you have the following:

1. A virtual machine with Docker installed and running.
2. Docker Compose (optional but recommended) to manage multi-container applications.

Step 1: Create Docker Containers

Start by creating individual Docker containers for each application. Each container should be configured to listen to a different port.

# Dockerfile for App1
FROM <base_image>
...
EXPOSE 8000
...
# Dockerfile for App2
FROM <base_image>
...
EXPOSE 9000
...

Step 2: Build and Run Containers

Build the Docker images for each application using the respective Dockerfiles. Then, run the containers, mapping the exposed container ports to host machine ports.

# Build and run App1 container
docker build -t app1-image .
docker run -d -p 8000:8000 --name app1-container app1-image
# Build and run App2 container
docker build -t app2-image .
docker run -d -p 9000:9000 --name app2-container app2-image

Step 3: Verify Container Accessibility

Ensure that each application is accessible by testing the URLs with their respective ports:

curl http://localhost:8000
curl http://localhost:9000
The above should respond with the application landing pages(HTML).

If you want to make the application available to the world over the public internet on custom ports, you will have to allow these ports over the firewalls (ex. security groups).

The best practice is to use NGINX as a reverse proxy and make the application available over the internet on port 80 or 443. Follow this blog for details

Step 4: Use Docker Compose (Optional)

If you prefer managing multiple containers using a single configuration file, Docker Compose can simplify the process.

Create a docker-compose.yml file:

version: '3'
services:
  app1:
    build:
      context: .
      dockerfile: Dockerfile.app1
    ports:
      - 8000:8000
  app2:
    build:
      context: .
      dockerfile: Dockerfile.app2
    ports:
      - 9000:9000
Run the containers using Docker Compose:

docker-compose up -d
In this blog post, we have learned how to host multiple Dockerized applications inside a single VM, allowing each application to listen on different ports. By creating separate Docker containers and mapping their exposed ports to the host machine, you can easily manage and access multiple applications concurrently. Docker Compose provides an additional layer of convenience for managing multi-container applications.

Remember to adapt the code samples to your specific application and port requirements. With these techniques, you can efficiently utilize resources within your VM while hosting multiple Dockerized applications.
