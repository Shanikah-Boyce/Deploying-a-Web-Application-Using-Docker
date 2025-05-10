# Deploying-a-Web-Application-Using-Docker
## What is Docker?
Docker is a powerful platform that simplifies application deployment by packaging software and its dependencies into lightweight, portable containers. These containers ensure consistency across various environments, from local development machines to cloud-based servers, making them an essential tool for modern software deployment.
![image](https://github.com/user-attachments/assets/96f0ecb0-a4ca-4656-b371-e5d9cc42beec)

## Project Overview
In this project, I containerized a simple web application using Docker and deployed it to the cloud using AWS Elastic Beanstalk. The process involved:
- Writing a custom Dockerfile
- Building a Docker image
- Running it locally
- Deploying it to a cloud-based environment with minimal friction

By leveraging Docker’s capabilities, I achieved a streamlined deployment pipeline that is both scalable and efficient.

## Why Use Docker?
Containers are lightweight, standalone software packages that include everything needed for an application to run, such as code, libraries, and tools. They are faster and more efficient than traditional virtual machines because they share the host system's kernel.

Docker simplifies container management with several key components:
- Docker Engine (daemon) – The background service that builds, runs, and manages containers.
- Docker CLI & API – Command-line tools and programmatic interfaces for interacting with Docker.
- Docker Desktop – A user-friendly GUI for managing containers, images, and volumes locally, especially useful for development and testing.

## Project Breakdown
### Running a Prebuilt Nginx Container
To get started, I experimented with a basic Nginx container to understand Docker’s core functionality: `docker run -d -p 80:80 nginx`
This command launched a Nginx server accessible via http://localhost, serving its default welcome page.
![image](https://github.com/user-attachments/assets/58f55b3e-d8af-418a-98cd-de51c289bda6)

## Creating a Custom Docker Image
Next, I built a custom Docker image within Dockerfile that served a personalized HTML page using Nginx.
![image](https://github.com/user-attachments/assets/65f8c0e6-00b0-48c0-8e98-52722607f167)

The Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. It's essentially a script of instructions for Docker to build a container image. My Dockerfile tells Docker 3 things: 
1)	FROM nginx:latest: Uses the latest Nginx image as the base.
2)	COPY index.html ...: Replaces the default page with my own.
3)	EXPOSE 80: Makes port 80 accessible from outside the container.
The command I used to build a custom image with my Dockerfile was `docker build -t my-web-app.` The '.' at the end of the command means that Docker should look for the Dockerfile in the current directory.
docker build -t my-web-app .
This created a local image named my-web-app.

## Running the Custom Image
To run the image, I used:
docker run -d -p 80:80 my-web-app
Initially, I encountered a port conflict because another container was using port 80. Using Docker Desktop, I identified and stopped the conflicting container, then successfully started mine. The custom page displayed "Welcome to My First Custom Docker Image!" at http://localhost.
![image](https://github.com/user-attachments/assets/8dab65f6-6b40-4df0-8a3c-f457820883e3)

## Cloud Deployment with AWS Elastic Beanstalk
To deploy my containerized app to the cloud, I used AWS Elastic Beanstalk, a Platform-as-a-Service (PaaS) that simplifies cloud application management. Here's what made it ideal:
•	Native Docker support
•	Automatic environment provisioning
•	Scalable infrastructure without manual configuration
After preparing a simple Dockerrun.aws.json file (or using a single-container setup), I uploaded my image to Elastic Beanstalk, and within minutes, my application was live in a production-grade environment.
![image](https://github.com/user-attachments/assets/a32a5771-63ee-4842-a3cf-ca1e05561f32)

## Unexpected Challenge: Virtualization Issues
A significant hurdle I faced was related to nested virtualization. My system could not support KVM when running Docker inside a virtual machine, leading to persistent errors. To resolve this:
1.	I installed Ubuntu directly on my machine.
2.	I set up Docker Desktop via Windows Subsystem for Linux (WSL).
This workaround allowed Docker to function correctly and enabled me to proceed with development and deployment without further issues.

## Time Investment
I completed the core tasks of this project—containerization, local testing, and cloud deployment—in about one hour. Most of the time was spent troubleshooting the virtualization problem, but the overall workflow was efficient thanks to Docker’s and AWS’s user-friendly tooling.

## Key Takeaways
•	Docker provides a seamless way to package and deploy applications across environments.
•	Creating custom Docker images is straightforward with a basic understanding of Dockerfiles.
•	AWS Elastic Beanstalk offers an easy on-ramp to cloud deployment for Dockerized apps.
•	Infrastructure quirks (like nested virtualization) can present real challenges—but modern solutions like WSL can mitigate them.


