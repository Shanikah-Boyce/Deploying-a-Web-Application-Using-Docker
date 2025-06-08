# Containerizing a Web Application with Docker and AWS Elastic Beanstalk

## Project Overview
This project involved containerizing a simple web application using Docker and deploying it to the cloud with AWS Elastic Beanstalk. 
By creating a custom Dockerfile, the application's environment and dependencies were properly defined. A Docker image was then built and tested locally to ensure functionality before deployment. 
AWS Elastic Beanstalk streamlined the cloud deployment process, making it easier to manage and scale the application. This approach aimed to ensure consistency, scalability, and ease of management across different environments, allowing for a seamless transition from local development to cloud servers.

## Understanding Containers and Docker
### Containers
Containers are software units that package an application and its dependencies. They are useful because they ensure consistent behavior across environments, isolate the application, eliminate compatibility issues, and simplify deployment.
A container image is a blueprint for creating containers, packaging everything needed to run an application.

### Docker
Docker is a platform for running applications in isolated containers with all their dependencies. Docker Desktop is a user-friendly application for managing these containers and images on your local computer through a graphical interface. The Docker daemon is the background engine that manages your Docker containers and images. It listens for Docker API requests and carries out the instructions to create, run, and manage containers. It's the core process that makes Docker work.

### Running a Prebuilt Nginx Container
To get started, I experimented with a basic Nginx container to understand Docker’s core functionality: `docker run -d -p 80:80 nginx`
This command launched a Nginx server accessible via http://localhost, serving its default welcome page.
![image](https://github.com/user-attachments/assets/58f55b3e-d8af-418a-98cd-de51c289bda6)

## Creating a Custom Docker Image
Next, I built a custom Docker image using a Dockerfile to serve a personalized HTML page with Nginx.
![image](https://github.com/user-attachments/assets/65f8c0e6-00b0-48c0-8e98-52722607f167)

A Dockerfile is essentially a set of instructions that guide Docker in assembling a container image. In this case, my Dockerfile included three essential steps:
1) `FROM nginx:latest` - Uses the latest Nginx image as the foundation.
2)	`COPY index.html ...` - Replaces the default homepage with my custom HTML file.
3)	`EXPOSE 80`- Opens port 80 for external access.

To build the image, I executed: `docker build -t my-web-app .` The dot (.) at the end tells Docker to locate the Dockerfile in the current directory. As a result, a local image named my-web-app was successfully created and ready for deployment.

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


