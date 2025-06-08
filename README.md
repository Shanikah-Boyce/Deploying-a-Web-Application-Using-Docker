# Containerizing a Web Application with Docker and AWS Elastic Beanstalk
<p align="center">
  <img src="https://github.com/user-attachments/assets/03e8fde8-daea-4a88-84c9-f9116cdcc386">
</p>

## Project Overview
In this project, I set out to containerize a simple web application using Docker and deploy it to the cloud with AWS Elastic Beanstalk. The goal was to achieve a repeatable, scalable deployment process that moves seamlessly from local development to a live production environment.

#### Why Containers? Why Docker?
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

## Deploying the Containerized Application with AWS Elastic Beanstalk

After successfully running the custom container locally, the next step involved deployment using AWS Elastic Beanstalk. This service significantly simplifies the management of containerized applications, offering support for Docker, automatic resource adjustment, and rapid deployment with minimal configuration.

To begin, an Elastic Beanstalk environment was configured with default settings. The application was named "NextWork App," and Docker was chosen as the platform, allowing Elastic Beanstalk to automatically determine the correct branch and version. Before uploading the application, the `index.html` file was updated to display a confirmation message, verifying a successful deployment.

A ZIP file containing both the `Dockerfile` and the updated `index.html` was created and uploaded to Elastic Beanstalk, labeled as "Version One." For this initial deployment and to remain within the free tier, the Single instance preset was selected, which is ideal for testing purposes.

For service access, a new IAM service role was generated, and `ecsInstanceRole` was chosen to grant the necessary permissions to the EC2 instance. In the networking section, the Public IP option was activated, while the default Virtual Private Cloud (VPC) settings were retained.

On the configuration page, General Purpose 3 (SSD) was selected for the root volume, with a size of 10GB. IMDSv1 was deactivated since the application did not require access to other AWS services. Basic monitoring was enabled, and managed updates were disabled given the temporary nature of the project.

The default "All at once" deployment strategy was used to ensure a swift setup. After reviewing all selections, the Elastic Beanstalk setup was finalized. Within minutes, the application was live and accessible.

![image](https://github.com/user-attachments/assets/a32a5771-63ee-4842-a3cf-ca1e05561f32)

## Troubleshooting and Security Considerations
Running Docker Desktop in a virtual machine caused nested virtualization issues, leading to KVM errors that blocked installation. Switching to WSL (Windows Subsystem for Linux) resolved the problem but introduced security concerns since it shares more with my main computer than a full virtual machine. To maintain a secure setup, regular updates, careful permission management and tools like Microsoft Defender for Endpoint and Intune are essential to keep my computer and data safe.

## Conclusion
By combining Docker’s containerization capabilities with AWS Elastic Beanstalk’s automated cloud management, this project successfully delivered a scalable, maintainable and efficient application infrastructure. The approach streamlined transitions between environments, making development, testing, and production workflows more seamless. Looking ahead, implementing a robust CI/CD pipeline will further automate and enhance the deployment process, paving the way for faster and more reliable software delivery.

