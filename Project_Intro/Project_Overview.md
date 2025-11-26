# MFIT Project Overview

The MFIT application is a web-based platform built using a microservices architecture to automate the Online Orientation Booking and Tracking process. It allows organizations to manage orientation sessions for new users efficiently, where users can book slots, receive notifications, and track their onboarding progress.

Here the frontend was developed using React with Bootstrap, and the backend consisted of multiple microservices built using Java Spring Boot. The communication between the frontend and backend was done through Axios, while communication between the backend microservices happened using Feign Client. We used PostgreSQL as the main database, and each backend service interacted with the database through Spring Data JPA repositories.

For deployment, we implemented a complete CI/CD pipeline covering code clone, build, test, scanning, and deployment. The tools used included Git, Maven, SonarQube, Trivy, Docker, and Jenkins.

We followed a hybrid cloud model. All application containers frontend and all backend microservices along with Nginx were hosted on our on-prem Ubuntu server. The rest of the DevOps tools such as Jenkins Master, SonarQube, and Trivy were hosted in AWS. To support this setup, we ran a Jenkins agent on-prem and connected it to the Jenkins Master in AWS using JNLP. This allowed pipelines to run builds and deployments locally on the on-prem server while the orchestration and scanning tools operated from the cloud.

We used Nginx as the reverse proxy, and the Nginx configuration file defined all application API routes to ensure incoming requests were forwarded to the correct microservice container.

Overall, this architecture allowed us to use cloud scalability for CI/CD and security tools, while keeping the actual application runtime and API routing securely on-prem.
