# Containerizing a Web Application with Docker and AWS Elastic Beanstalk
<p align="center">
  <img src="https://github.com/user-attachments/assets/e2a70a77-6977-4155-9dd2-df2f049bed5d">
</p>

## Project Overview
In this project, I set out to containerize a simple web application using Docker and deploy it to the cloud with AWS Elastic Beanstalk. The goal was to achieve a repeatable, scalable deployment process that moves seamlessly from local development to a live production environment.

## Why Containers? Why Docker?
Containers offer a way to package applications along with their dependencies so they behave the same in any environment (local, testing, or production). This level of consistency reduces bugs, eliminates "it works on my machine" problems, and simplifies deployment. 
Thus, Docker is a popular platform for building and managing containers. It allows developers to:
- Run applications in isolated, lightweight environments
- Define software environments with simple configuration files
- Easily test and share containerized applications

Unlike traditional virtual machines, containers share the host OS, making them significantly lighter and faster to start up.

## Containerization Workflow
### Running a Prebuilt Nginx Container
To explore Docker, I began with a basic Nginx container: `docker run -d -p 80:80 nginx`. This started a local web server accessible via http://localhost, displaying the default Nginx welcome page.

![image](https://github.com/user-attachments/assets/58f55b3e-d8af-418a-98cd-de51c289bda6)

#### Why Nginx?
Nginx is lightweight, highly scalable and optimized for serving static files and reverse proxying. It was a natural choice for delivering web content efficiently.

### Creating a Custom Docker Image
Next, I created a custom Dockerfile to serve a personalized HTML page through Nginx:

`FROM nginx: latest` - Uses the latest Nginx image as the foundation.

`COPY index.html /usr/share/nginx/html/index.html` - Replaces the default homepage with my custom HTML file.

`EXPOSE 80` - Opens port 80 for external access.

I then built the image with: `docker build -t my-web-app .` The `.` tells Docker to look for the Dockerfile in the current directory. Once built, I ran: `docker run -d -p 80:80 my-web-app`.
An existing container was already using port 80, so I stopped it using Docker Desktop. After that, my custom page loaded successfully in the browser:

![image](https://github.com/user-attachments/assets/8dab65f6-6b40-4df0-8a3c-f457820883e3)

## Taking It to the Cloud: AWS Elastic Beanstalk
With the custom container working locally, I deployed it using AWS Elastic Beanstalk, a service that simplifies setting up and managing containerized applications. It was a great choice because it:
- Naturally supports Docker out-of-the-box
- Automatically adjusts resources
- Quickly gets applications ready for use with minimal setup

## Deployment Steps
To deploy, I followed these steps:
- Containerized the application using the Dockerfile.
- Uploaded the Docker image to AWS Elastic Container Registry (ECR).
- Configured an Elastic Beanstalk environment, specifying a Docker-based setup.
- Deployed via the Elastic Beanstalk CLI (eb init, eb create).
- Monitored logs and resource scaling to ensure smooth operation.

Within minutes, the app was live on the internet.

![image](https://github.com/user-attachments/assets/a32a5771-63ee-4842-a3cf-ca1e05561f32)

## Troubleshooting and Security Considerations
Running Docker Desktop in a virtual machine caused nested virtualization issues, leading to KVM errors that blocked installation. Switching to WSL (Windows Subsystem for Linux) resolved the problem but introduced security concerns since it shares more with my main computer than a full virtual machine. To maintain a secure setup, regular updates, careful permission management and tools like Microsoft Defender for Endpoint and Intune are essential to keep my computer and data safe.

## Conclusion
By combining Docker’s containerization capabilities with AWS Elastic Beanstalk’s automated cloud management, this project successfully delivered a scalable, maintainable and efficient application infrastructure. The approach streamlined transitions between environments, making development, testing, and production workflows more seamless. Looking ahead, implementing a robust CI/CD pipeline will further automate and enhance the deployment process, paving the way for faster and more reliable software delivery.

