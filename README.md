

Jenkins Setup with Docker and Docker Compose
This README provides detailed instructions for setting up Jenkins using Docker and Docker Compose. Additionally, it includes steps for configuring a Jenkins pipeline to display Node.js and Maven versions.

Prerequisites
Ubuntu OS
Docker installed
Docker Compose installed
Steps
1. Install Docker
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
2. Install Docker Compose
sudo apt install docker-compose -y
3. Set Up Jenkins Using Docker Compose
Create a docker-compose.yaml file with the following content:

version: '3.8'
services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home

volumes:
  jenkins_home:
Run Docker Compose to start Jenkins:

docker-compose up -d
4. Access Jenkins
Open a web browser and navigate to http://your_server_ip_or_domain:8080.

5. Retrieve Initial Admin Password
Run the following command to get the initial admin password:

docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
Use the retrieved password to unlock Jenkins.

6. Install Docker Plugin in Jenkins
Go to Manage Jenkins -> Manage Plugins -> Available.
Search for Docker Pipeline and install it.
7. Create Jenkins Pipeline Job
Go to New Item, enter a name, select Pipeline, and click OK.
In the pipeline configuration, scroll down to the Pipeline section and select Pipeline script.
8. Add Pipeline Script
pipeline {
    agent any

    stages {
        stage('Build Node.js Container') {
            steps {
                script {
                    docker.build('zubair/node:14.1', '-f Dockerfile.node .').inside {
                        sh 'node -v'
                        sh 'echo Node $(node -v) - Zubair'
                    }
                }
            }
        }
        stage('Build Maven Container') {
            steps {
                script {
                    docker.build('zubair/maven:3.8.1', '-f Dockerfile.maven .').inside {
                        sh 'mvn -v'
                        sh 'echo Maven $(mvn -v) - Zubair'
                    }
                }
            }
        }
    }
}
9. Create Dockerfiles
Create Dockerfile.node in your Jenkins workspace:

# Dockerfile for Node.js
FROM node:14.1
RUN echo "Node $(node -v) - Zubair"
Create Dockerfile.maven in your Jenkins workspace:

# Dockerfile for Maven
FROM maven:3.8.1-jdk-11
RUN echo "Maven $(mvn -v) - Zubair"
10. Run the Pipeline
Save the job configuration.
Build the pipeline job.
This setup will build Docker images for Node.js and Maven, and the pipeline will display the versions along with the name "Zubair".

Troubleshooting
Conflict with Existing Container Names
If you encounter an error indicating that the container name is already in use, remove the conflicting container:

docker rm <container_id>
Or run the new container with a different name.

Cleaning Up Docker
To clean up Docker system resources, use:

docker system prune -a --volumes
Conclusion
This guide covers the setup of Jenkins using Docker and Docker Compose, and the configuration of a Jenkins pipeline to display Node.js and Maven versions. If you encounter any issues, refer to the troubleshooting section or consult the Docker and Jenkins documentation for further assistance.

This README should provide a clear and comprehensive guide for setting up Jenkins with Docker and Docker Compose, configuring the pipeline, and troubleshooting common issues.

Jenkins Setup with Docker and Docker Compose
============================================

This README provides detailed instructions for setting up Jenkins using Docker and Docker Compose. Additionally, it includes steps for configuring a Jenkins pipeline to display Node.js and Maven versions.

Prerequisites
-------------

-   Ubuntu OS
-   Docker installed
-   Docker Compose installed

Steps
-----

### 1\. Install Docker

`sudo apt update sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker`

### 2\. Install Docker Compose

`sudo apt install docker-compose -y `

### 3\. Set Up Jenkins Using Docker Compose

Create a `docker-compose.yaml` file with the following content:

`version:  '3.8'  services:   jenkins:   image:  jenkins/jenkins:lts   container_name:  jenkins   ports:   -  "8080:8080"   -  "50000:50000"   volumes:   -  jenkins_home:/var/jenkins_home
volumes:   jenkins_home:  `

Run Docker Compose to start Jenkins:

`docker-compose up -d `

### 4\. Access Jenkins

Open a web browser and navigate to `http://your_server_ip_or_domain:8080`.

### 5\. Retrieve Initial Admin Password

Run the following command to get the initial admin password:

`docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword `

Use the retrieved password to unlock Jenkins.

### 6\. Install Docker Plugin in Jenkins

1.  Go to `Manage Jenkins` -> `Manage Plugins` -> `Available`.
2.  Search for `Docker Pipeline` and install it.

### 7\. Create Jenkins Pipeline Job

1.  Go to `New Item`, enter a name, select `Pipeline`, and click `OK`.
2.  In the pipeline configuration, scroll down to the Pipeline section and select `Pipeline script`.

### 8\. Add Pipeline Script

`pipeline {
    agent any

    stages {
 stage('Build Node.js Container') {
            steps {
                script {
 docker.build('zubair/node:14.1', '-f Dockerfile.node .').inside {
 sh 'node -v'  sh 'echo Node $(node -v) - Zubair'                     }
                }
            }
        }
 stage('Build Maven Container') {
            steps {
                script {
 docker.build('zubair/maven:3.8.1', '-f Dockerfile.maven .').inside {
 sh 'mvn -v'  sh 'echo Maven $(mvn -v) - Zubair'                     }
                }
            }
        }
    }
}`

### 9\. Create Dockerfiles

-   Create `Dockerfile.node` in your Jenkins workspace:

    `# Dockerfile for Node.js  FROM node:14.1  RUN  echo  "Node $(node -v) - Zubair"  `

-   Create `Dockerfile.maven` in your Jenkins workspace:

    `# Dockerfile for Maven  FROM maven:3.8.1-jdk-11  RUN  echo  "Maven $(mvn -v) - Zubair"  `

### 10\. Run the Pipeline

1.  Save the job configuration.
2.  Build the pipeline job.

This setup will build Docker images for Node.js and Maven, and the pipeline will display the versions along with the name "Zubair".

Troubleshooting
---------------

### Conflict with Existing Container Names

If you encounter an error indicating that the container name is already in use, remove the conflicting container:

`docker rm <container_id> `

Or run the new container with a different name.

### Cleaning Up Docker

To clean up Docker system resources, use:

`docker system prune -a --volumes `

Conclusion
----------

This guide covers the setup of Jenkins using Docker and Docker Compose, and the configuration of a Jenkins pipeline to display Node.js and Maven versions. If you encounter any issues, refer to the troubleshooting section or consult the Docker and Jenkins documentation for further assistance.

* * * * *

This README should provide a clear and comprehensive guide for setting up Jenkins with Docker and Docker Compose, configuring the pipeline, and troubleshooting common issues.
