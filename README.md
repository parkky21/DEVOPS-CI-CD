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
3. [Setup & Run Instructions](#setup--run-instructions)
4. [Docker Commands](#docker-commands)
5. [Sample Test Output](#sample-test-output)
6. [Team Members and Responsibilities](#team-members-and-responsibilities)

## Project Description

This project is a modern, responsive web application inspired by Zentry's design patterns. It features scroll-triggered animations, geometric transitions, and engaging UI/UX with smooth responsiveness. Built using React.js, GSAP for animations, and Tailwind CSS for styling, the application demonstrates best practices in modern web development.

## CI/CT/CD Pipeline Flow

Our implementation uses a comprehensive CI/CT/CD pipeline for automated testing, building, and deployment:

```
┌─────────────┐     ┌─────────────┐     ┌────────────────┐     ┌────────────┐     ┌──────────────┐
│ Code Commit ├────►│ Unit Tests  ├────►│ Integration    ├────►│ Build      ├────►│ Deployment   │
│ & Push      │     │             │     │ Tests          │     │ Docker     │     │ to Production│
└─────────────┘     └─────────────┘     └────────────────┘     └────────────┘     └──────────────┘
       │                   │                    │                    │                    │
       │                   │                    │                    │                    │
       ▼                   ▼                    ▼                    ▼                    ▼
┌─────────────┐     ┌─────────────┐     ┌────────────────┐     ┌────────────┐     ┌──────────────┐
│ Linting &   │     │ Test        │     │ Test Coverage  │     │ Security   │     │ Performance  │
│ Code Quality│     │ Reports     │     │ Reports        │     │ Scans      │     │ Monitoring   │
└─────────────┘     └─────────────┘     └────────────────┘     └────────────┘     └──────────────┘
```

Pipeline Steps:
1. Code is committed and pushed to the repository
2. Automated linting and code quality checks run
3. Unit tests execute to verify component functionality
4. Integration tests ensure components work together
5. Test coverage reports are generated
6. Docker image is built and tagged
7. Security scans run against the Docker image
8. Docker image is deployed to production environment
9. Performance monitoring begins for the new deployment

## Setup & Run Instructions

### Prerequisites

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en) (v16 or higher)
- [npm](https://www.npmjs.com/) (v7 or higher)
- [Docker](https://www.docker.com/) (optional for containerized deployment)

### Local Development Setup

```bash
# Clone the repository
git clone https://github.com/your-username/zentry-inspired-app.git
cd zentry-inspired-app

# Install dependencies
npm install

# Run development server
npm run dev
```

The application will be available at [http://localhost:5173](http://localhost:5173)

### Running Tests

```bash
# Run unit tests
npm test

# Run tests with coverage
npm run test:coverage
```

## Docker Commands

### Building Docker Image

```bash
# Build image
docker build -t zentry-app:latest .

# Run container
docker run -p 8090:8090 zentry-app:latest
```

### Using Docker Compose

```bash
# Start services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
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