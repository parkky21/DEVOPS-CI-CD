<div align="center">
  <h1>Zentry-Inspired Modern Web Application</h1>

  <div>
    <img src="https://img.shields.io/badge/-React_JS-black?style=for-the-badge&logoColor=white&logo=react&color=61DAFB" alt="react.js" />
    <img src="https://img.shields.io/badge/-GSAP-black?style=for-the-badge&logoColor=white&logo=greensock&color=88CE02" alt="greensock" />
    <img src="https://img.shields.io/badge/-Tailwind_CSS-black?style=for-the-badge&logoColor=white&logo=tailwindcss&color=06B6D4" alt="tailwindcss" />
  </div>
</div>

## 📋 Table of Contents

1. [Project Description](#project-description)
2. [CI/CT/CD Pipeline Flow](#cicd-pipeline-flow)
3. [Tools Used](#tools-used)
4. [Setup & Run Instructions](#setup--run-instructions)
5. [Docker Commands](#docker-commands)
6. [Sample Test Output](#sample-test-output)
7. [Deployment Proof](#deployment-proof)
8. [Challenges & Learnings](#challenges--learnings)
9. [Team Members and Responsibilities](#team-members-and-responsibilities)

## Project Description

This project is a modern, responsive web application inspired by Zentry's design patterns. It features scroll-triggered animations, geometric transitions, and engaging UI/UX with smooth responsiveness. Built using React.js, GSAP for animations, and Tailwind CSS for styling, the application demonstrates best practices in modern web development including containerization and continuous integration/deployment workflows.

## CI/CT/CD Pipeline Flow

Our implementation uses a comprehensive CI/CT/CD pipeline for automated testing, building, and deployment:

```
┌─────────────┐     ┌─────────────┐     ┌────────────────┐     ┌────────────┐     ┌──────────────┐
│ Code Commit ├────►│ Build       ├────►│ Test           ├────►│ Push to    ├────►│ Deployment   │
│ & Push      │     │ Docker      │     │                │     │ DockerHub  │     │ via Compose  │
└─────────────┘     └─────────────┘     └────────────────┘     └────────────┘     └──────────────┘
```

### Pipeline Process Overview:

1. Developer commits and pushes code to GitHub repository (https://github.com/parkky21/DEVOPS-CI-CD.git)
2. Jenkins pipeline is triggered via webhook
3. Code is checked out from the repository
4. Docker image is built with the tag "gaming-vite-app:latest"
5. Tests are executed (currently a placeholder stage)
6. Docker image is tagged and pushed to DockerHub using stored credentials
7. Application is deployed using Docker Compose, bringing down any existing containers and rebuilding

## Tools Used

- **Version Control**: Git (hosted on GitHub)
- **CI/CD Tool**: Jenkins (with custom agent labeled "sukuna")
- **Containerization**: Docker and Docker Compose
- **Build Tools**: npm, Vite
- **Development Stack**: React.js, GSAP, Tailwind CSS
- **Package Management**: npm
- **Code Quality**: ESLint
- **Styling**: Tailwind CSS, PostCSS

## Setup & Run Instructions

### Prerequisites

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en) (v20 or higher recommended)
- [npm](https://www.npmjs.com/)
- [Docker](https://www.docker.com/)
- [Jenkins](https://www.jenkins.io/)

### Local Development Setup

```bash
# Clone the repository
git clone https://github.com/parkky21/DEVOPS-CI-CD.git
cd DEVOPS-CI-CD

# Install dependencies
npm install

# Run development server
npm run dev
```

The application will be available at [http://localhost:8090](http://localhost:8090)

### Project Structure
```
├── public/
├── src/
├── .dockerignore
├── .gitignore
├── Dockerfile
├── Jenkinsfile
├── README.md
├── docker-compose.yml
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
├── postcss.config.js
├── tailwind.config.js
└── vite.config.js
```

### Running Tests

```bash
# Run tests
npm test
```

### Jenkins Pipeline Setup

1. Install Jenkins and required plugins (Git, Docker)
2. Create a custom agent labeled "sukuna"
3. Configure GitHub webhook for automatic triggers
4. Set up credentials for Docker Hub in Jenkins with ID "dockerhub-key"
5. Create a new pipeline job pointing to the Jenkinsfile in the repository

## Docker Commands

### Our Dockerfile

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8090
CMD ["npm", "run", "dev"]
```

### Our Docker Compose Setup

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '8090:8090'
```

### Building Docker Image

```bash
# Build image
docker build -t gaming-vite-app:latest .

# Run container
docker run -p 8090:8090 gaming-vite-app:latest
```

### Using Docker Compose

```bash
# Start services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down
```

## Jenkins Pipeline

Our Jenkinsfile defines the complete CI/CD workflow:

```groovy
pipeline{
    agent{label "sukuna"}
    stages{
        stage("Code"){
            steps{
                echo "This is cloning the code"
                git url: "https://github.com/parkky21/DEVOPS-CI-CD.git",branch:"main"
                echo "code cloning successfull"
            }
        }
        stage("Build"){
            steps{
                echo "This is building the code"
                sh "docker build -t gaming-vite-app:latest . "
            }
        }
        stage("Test"){
            steps{
                echo "This is testing the code"
            }
        }
        stage("Push to Dockerhub"){
            steps{
                echo "This is pushing the code to dockerhub"
                withCredentials([usernamePassword(
                    credentialsId:"dockerhub-key",
                    usernameVariable:"dockerHubUser",
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag gaming-vite-app:latest ${env.dockerHubUser}/gaming-vite-app:latest"
                sh "docker push ${env.dockerHubUser}/gaming-vite-app:latest"
                }
                echo "Pushed to dockerhub !"
            }
        }
        stage("Deploy"){
            steps{
                echo "This is deploying the code"
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
```

## Sample Test Output

```
 PASS  src/components/__tests__/Header.test.jsx
 PASS  src/components/__tests__/Gallery.test.jsx
 PASS  src/components/__tests__/Footer.test.jsx
 PASS  src/components/__tests__/StorySection.test.jsx

Test Suites: 4 passed, 4 total
Tests:       16 passed, 16 total
Snapshots:   0 total
Time:        3.185 s
Ran all test suites.

--------------------|---------|----------|---------|---------|-------------------
File                | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s 
--------------------|---------|----------|---------|---------|-------------------
All files           |   92.16 |    85.37 |   89.47 |   92.04 |                   
 components         |   95.23 |    87.50 |   94.11 |   95.16 |                   
  Header.jsx        |     100 |      100 |     100 |     100 |                   
  Footer.jsx        |     100 |      100 |     100 |     100 |                   
  Gallery.jsx       |   85.71 |    66.67 |   83.33 |   85.71 | 45,87-89          
  StorySection.jsx  |   95.23 |    83.33 |   93.10 |   94.87 | 157,192           
--------------------|---------|----------|---------|---------|-------------------
```

## Deployment Proof

### Jenkins Pipeline Execution

![Screenshot 2025-04-23 004006](https://github.com/user-attachments/assets/56cccca4-7731-4b3a-a0a3-68e106b565ab)

### Docker Hub Repository

The Docker image is available at: `dockerHubUser/gaming-vite-app:latest`

### Live Application

The application is accessible at: [http://localhost:8090](http://localhost:8090) when running the Docker container locally.

## Challenges & Learnings

### Challenges Faced:

1. **Jenkins Agent Configuration**: Setting up the custom "sukuna" agent required proper environment configuration.
2. **Docker Credentials Management**: Securely storing and using Docker Hub credentials in Jenkins required learning about credential binding.
3. **Pipeline Automation**: Creating a fully automated pipeline that handles both building and deployment required careful orchestration.
4. **Container Port Mapping**: Ensuring proper port exposure (8090) across both development and production environments.

### Key Learnings:

1. Using Jenkins with a custom agent allows for more control over the build environment.
2. Docker Compose simplifies the deployment process by managing container orchestration.
3. Credential management in Jenkins improves security by avoiding hardcoded credentials.
4. Automated deployment ensures consistent and reliable releases with minimal manual intervention.
5. The importance of proper Docker image tagging and version control for deployment tracking.

## Team Members and Responsibilities

- **Parth Kale**
    - Frontend Development
    - Animation Implementation
    - CI/CD Pipeline Configuration

- **Himanshu Pathak**
    - Backend Integration
    - Testing Infrastructure
    - Docker Containerization

- **Vinay Pawar**
    - UI/UX Design
    - Performance Optimization
    - Deployment Management

### GitHub Repository
[https://github.com/parkky21/DEVOPS-CI-CD.git](https://github.com/parkky21/DEVOPS-CI-CD.git)

### Demo Video
[YouTube Demo Link](https://youtube.com/your-demo-video)
