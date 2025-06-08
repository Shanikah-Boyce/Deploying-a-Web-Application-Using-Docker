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

![image](https://github.com/user-attachments/assets/8dab65f6-6b40-4df0-8a3c-f457820883e3)

## Taking It to the Cloud: AWS Elastic Beanstalk
With the custom container working locally, I deployed it using AWS Elastic Beanstalk, a platform-as-a-service that automates infrastructure provisioning and deployment for containerized applications.

What made Elastic Beanstalk ideal:
- Built-in support for Docker containers
- Automated provisioning and scaling
- Fast setup for production environments

Deployment was straightforward. After uploading the Docker image, the platform handled the rest. Within minutes, the app was live and accessible on the internet.
![image](https://github.com/user-attachments/assets/a32a5771-63ee-4842-a3cf-ca1e05561f32)

## Troubleshooting: Virtualization Woes
One challenge I faced was nested virtualization. Running Docker inside a virtual machine caused KVM errors and made containers fail to start.

My Fix:
- I switched to running Ubuntu natively on my system.
- Alternatively, I configured Docker Desktop with WSL (Windows Subsystem for Linux).
Both approaches resolved the issue and allowed Docker to function normally.

## Key Takeaways
- Docker is incredibly effective for creating portable, consistent application environments.
- Writing a basic Dockerfile and building custom images is surprisingly easy.
- AWS Elastic Beanstalk streamlines container deployment without needing to manage infrastructure.
- System limitations like virtualization support can slow progress but are often solvable with the right setup.

## Conclusion
This project demonstrated how modern tooling like Docker and AWS Elastic Beanstalk can simplify web application deployment. By containerizing the app, I was able to ensure consistency across environments. Deploying to the cloud was quick, reliable, and scalable—everything needed for modern app delivery, with minimal manual configuration.
