# Containerizing a Web Application with Docker and AWS Elastic Beanstalk

## Project Overview
In this project, I set out to containerize a simple web application using Docker and deploy it to the cloud with AWS Elastic Beanstalk. The goal was to achieve a repeatable, scalable deployment process that moves seamlessly from local development to a live production environment.

## Why Containers? Why Docker?
Containers offer a way to package applications along with their dependencies so they behave the same in any environment (local, testing, or production). This level of consistency reduces bugs, eliminates "it works on my machine" problems, and simplifies deployment. Thus, Docker is a popular platform for building and managing containers. It allows developers to:
- Run applications in isolated, lightweight environments
- Define software environments with simple configuration files
- Easily test and share containerized applications

## Containerization Workflow
### Running a Prebuilt Nginx Container
To explore Docker, I began with a basic Nginx container: `docker run -d -p 80:80 nginx`. This started a local web server accessible via http://localhost, displaying the default Nginx welcome page.
![image](https://github.com/user-attachments/assets/58f55b3e-d8af-418a-98cd-de51c289bda6)

## Creating a Custom Docker Image
Next, I created a custom Dockerfile to serve a personalized HTML page through Nginx:

`FROM nginx: latest` - Uses the latest Nginx image as the foundation.

`COPY index.html /usr/share/nginx/html/index.html` - Replaces the default homepage with my custom HTML file.

`EXPOSE 80` - Opens port 80 for external access.

I then built the image with: `docker build -t my-web-app .` The `.` tells Docker to look for the Dockerfile in the current directory. Once built, I ran: `docker run -d -p 80:80 my-web-app`.
An existing container was already using port 80, so I stopped it using Docker Desktop. After that, my custom page loaded successfully in the browser:

## Running the Custom Image
To run the image, I used:
docker run -d -p 80:80 my-web-app
Initially, I encountered a port conflict because another container was using port 80. Using Docker Desktop, I identified and stopped the conflicting container, then successfully started mine. The custom page displayed "Welcome to My First Custom Docker Image!" at http://localhost.
![image](https://github.com/user-attachments/assets/8dab65f6-6b40-4df0-8a3c-f457820883e3)

## Cloud Deployment with AWS Elastic Beanstalk
To deploy my containerized app to the cloud, I used AWS Elastic Beanstalk, a Platform-as-a-Service (PaaS) that simplifies cloud application management. Here's what made it ideal:
- Native Docker support
- Automatic environment provisioning
- Scalable infrastructure without manual configuration

After using a Dockerfile to locally build my image, I uploaded it to AWS Elastic Beanstalk. Within minutes, my application was live in a production-ready environment, fully managed and optimized for scaling.
![image](https://github.com/user-attachments/assets/a32a5771-63ee-4842-a3cf-ca1e05561f32)

## Unexpected Challenge: Virtualization Issues
A significant hurdle I faced was related to nested virtualization. My system could not support KVM when running Docker inside a virtual machine, leading to persistent errors. To resolve this:
1.	I installed Ubuntu directly on my machine.
2.	I set up Docker Desktop via Windows Subsystem for Linux (WSL).
This workaround allowed Docker to function correctly and enabled me to proceed with development and deployment without further issues.

## Time Investment
I completed the core tasks of this project (containerization, local testing, and cloud deployment) in about one hour. Most of the time was spent troubleshooting the virtualization problem, but the overall workflow was efficient thanks to Docker’s and AWS’s user-friendly tooling.

## Key Takeaways
- Docker provides a seamless way to package and deploy applications across environments.
- Creating custom Docker images is straightforward with a basic understanding of Dockerfiles.
- AWS Elastic Beanstalk offers an easy on-ramp to cloud deployment for Dockerized apps.
- Infrastructure quirks (like nested virtualization) can present real challenges—but modern solutions like WSL can mitigate them.# 


