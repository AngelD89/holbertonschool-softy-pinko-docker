# Softy Pinko Docker Project

A comprehensive Docker and Docker Compose project demonstrating containerization, microservices architecture, reverse proxying, and load balancing.

## Table of Contents

- [Project Overview](#project-overview)
- [Directory Structure](#directory-structure)
- [Tasks](#tasks)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Architecture](#architecture)
- [Technologies](#technologies)
- [License](#license)

## Project Overview

This project progressively builds a complete web application using Docker, starting with simple containers and advancing to a production-like architecture with load balancing. Each task builds upon the previous one, introducing new concepts and features.

### Key Features

- ✅ Static content serving with Nginx
- ✅ Python Flask API backend
- ✅ Cross-Origin Resource Sharing (CORS)
- ✅ Reverse proxy routing
- ✅ Load balancing with multiple API servers
- ✅ Docker Compose orchestration
- ✅ Service discovery and networking

## Directory Structure

```
.
├── task0/                    # Basic Hello World Docker container
│   └── Dockerfile
├── task1/                    # Flask API server
│   ├── Dockerfile
│   └── api.py
├── task2/                    # Separated front-end and back-end
│   ├── back-end/
│   │   ├── Dockerfile
│   │   └── api.py
│   └── front-end/
│       ├── Dockerfile
│       ├── softy-pinko-front-end.conf
│       └── softy-pinko-front-end/
│           └── index.html
├── task3/                    # Connected front-end and back-end
│   ├── back-end/
│   │   ├── Dockerfile
│   │   └── api.py            # Updated with CORS
│   └── front-end/
│       ├── Dockerfile
│       ├── softy-pinko-front-end.conf
│       └── softy-pinko-front-end/
│           └── index.html    # Updated with jQuery
├── task4/                    # Docker Compose orchestration
│   ├── docker-compose.yml
│   ├── back-end/
│   │   ├── Dockerfile
│   │   └── api.py
│   └── front-end/
│       ├── Dockerfile
│       ├── softy-pinko-front-end.conf
│       └── softy-pinko-front-end/
│           └── index.html
├── task5/                    # Reverse proxy server
│   ├── docker-compose.yml
│   ├── proxy/
│   │   ├── Dockerfile
│   │   └── proxy.conf
│   ├── back-end/
│   │   ├── Dockerfile
│   │   └── api.py
│   └── front-end/
│       ├── Dockerfile
│       ├── softy-pinko-front-end.conf
│       └── softy-pinko-front-end/
│           └── index.html
├── task6/                    # Load balancing with multiple API servers
│   ├── docker-compose.yml
│   ├── 2-api-servers.txt     # Command to run with 2 API servers
│   ├── proxy/
│   │   ├── Dockerfile
│   │   └── proxy.conf        # Updated with upstream load balancing
│   ├── back-end/
│   │   ├── Dockerfile
│   │   └── api.py
│   └── front-end/
│       ├── Dockerfile
│       ├── softy-pinko-front-end.conf
│       └── softy-pinko-front-end/
│           └── index.html
├── database.csv              # Sample student data
├── package.json              # Node.js project configuration
├── babel.config.js           # Babel ES6 transpilation config
├── .eslintrc.js              # ESLint code quality config
├── .babelrc                  # Babel preset configuration
└── README.md                 # This file
```

## Tasks

### Task 0: Basic Docker Container
- Creates a simple Ubuntu container that echoes "Hello, World!"
- **Port**: N/A
- **Run**: `docker build -f ./Dockerfile -t softy-pinko:task0 . && docker run softy-pinko:task0`

### Task 1: Flask API Server
- Sets up a Python Flask API server
- Installs Python3, pip3, and Flask
- **Port**: 5252
- **Endpoint**: `/api/hello` returns "Hello, World!"
- **Run**: `docker build -f ./Dockerfile -t softy-pinko:task1 ./task1 && docker run -p 5252:5252 softy-pinko:task1`

### Task 2: Separated Architecture
- Separates front-end (Nginx) and back-end (Flask)
- Front-end serves static content
- Back-end provides API
- **Ports**: Front-end 9000, Back-end 5252
- **Run**: Build and run separate containers for front-end and back-end

### Task 3: Connected Services
- Connects front-end and back-end with CORS support
- Front-end uses jQuery to fetch dynamic data from API
- Enables cross-origin requests with flask-cors
- **JavaScript**: Makes AJAX calls to `http://localhost:5252/api/hello`
- **Run**: Run both containers with port mappings

### Task 4: Docker Compose Orchestration
- Introduces Docker Compose for managing multiple containers
- Single command to start both services
- Automatic networking between services
- **Run**: `docker-compose build && docker-compose up`

### Task 5: Reverse Proxy Server
- Adds Nginx reverse proxy as single entry point
- Routes `/` to front-end on port 9000
- Routes `/api` to back-end on port 5252
- Front-end updated to call API through proxy (`/api/hello`)
- **Port**: 80 (proxy only exposed externally)
- **Run**: `docker-compose build && docker-compose up`

### Task 6: Load Balancing
- Scales to multiple API servers using Docker Compose
- Nginx load balances between API servers using Round-Robin
- Demonstrates horizontal scaling
- **Run**: `docker-compose up --scale back-end=2`
- Or use command from `2-api-servers.txt`

## Prerequisites

Before you begin, ensure you have the following installed:

- **Docker**: [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose**: [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Git**: For cloning and managing the repository
- **Text Editor**: VS Code, Sublime, or any text editor

### Verify Installation

```bash
docker --version
docker-compose --version
git --version
```

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/holbertonschool-softy-pinko-docker.git
cd holbertonschool-softy-pinko-docker
```

### 2. Navigate to a Task

```bash
# Example: Go to task6 (most advanced)
cd task6
```

### 3. Build the Images

```bash
docker-compose build
```

### 4. Start the Containers

For most tasks (4, 5):
```bash
docker-compose up
```

For task6 with load balancing (2 API servers):
```bash
docker-compose up --scale back-end=2
```

Or use the 5 API servers example:
```bash
docker-compose up --scale back-end=5
```

### 5. Access the Application

- **Front-end**: http://localhost (task5, task6) or http://localhost:9000 (task4)
- **API**: http://localhost/api/hello (through proxy in task5, task6)
- **Direct API** (task4): http://localhost:5252/api/hello

## Usage

### Basic Commands

```bash
# Build Docker images
docker-compose build

# Start containers in foreground
docker-compose up

# Start containers in background
docker-compose up -d

# View logs
docker-compose logs

# View logs for specific service
docker-compose logs back-end

# Stop containers
docker-compose down

# Scale a service
docker-compose up --scale back-end=3

# View running containers
docker-compose ps

# Execute command in container
docker-compose exec back-end bash
```

### Testing Load Balancing (Task 6)

1. Start the application with 2 API servers:
```bash
cd task6
docker-compose up --scale back-end=2
```

2. Open a browser and navigate to http://localhost

3. Reload the page multiple times (or check browser console)

4. Watch the terminal output - you'll see requests alternating between `back-end-1` and `back-end-2`:
```
task6-back-end-1   | 172.20.0.5 - - [15/Jun/2023 19:53:55] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-2   | 172.20.0.5 - - [15/Jun/2023 19:53:57] "GET /api/hello HTTP/1.0" 200 -
```

## Architecture

### Task 5 & 6 Architecture

```
┌─────────────────────────────────────────┐
│          Client (Browser)               │
│        http://localhost:80              │
└──────────────────┬──────────────────────┘
                   │
        ┌──────────▼──────────┐
        │   Proxy Server      │
        │  (Nginx on port 80) │
        │  Task5/Task6        │
        └──────────┬──────────┘
                   │
        ┌──────────┴──────────┐
        │                     │
   ┌────▼─────┐         ┌────▼──────┐
   │ Front-end │         │ Back-end  │
   │(Nginx)    │         │(Flask API)│
   │Port: 9000 │         │Port: 5252 │
   └────▲─────┘         └────▲──────┘
        │                    │
        │            ┌───────┴───────┐
        │            │               │
        │      ┌─────▼─────┐  ┌─────▼─────┐
        │      │ Back-end-1│  │ Back-end-2│
        │      │   (Task6) │  │   (Task6) │
        │      └───────────┘  └───────────┘
        │
   Route: /
   Route: /api ──────────────────────────┘
```

## Technologies

### Docker & Containerization
- **Docker**: Container runtime
- **Docker Compose**: Multi-container orchestration
- **Nginx**: Web server and reverse proxy
- **Ubuntu**: Base OS image

### Backend
- **Python 3**: Programming language
- **Flask**: Web framework
- **Flask-CORS**: Cross-Origin Resource Sharing
- **pip3**: Python package manager

### Frontend
- **Nginx**: Static content server
- **HTML/CSS/JavaScript**: Web interface
- **jQuery**: AJAX requests to backend

### Development Tools
- **Babel**: ES6 transpiler
- **ESLint**: Code linting
- **Jest**: Testing framework

## Key Concepts Covered

1. **Containerization**: Packaging applications in Docker containers
2. **Images vs Containers**: Understanding Docker images and running containers
3. **Port Mapping**: Exposing container ports to host machine
4. **Networking**: Service-to-service communication
5. **Docker Compose**: Orchestrating multiple containers
6. **Reverse Proxy**: Routing traffic to different services
7. **Load Balancing**: Distributing requests across multiple servers
8. **CORS**: Handling cross-origin requests
9. **Service Discovery**: Docker's internal DNS for service names
10. **Scaling**: Horizontal scaling with `--scale` flag

## Common Issues & Solutions

### Issue: Port Already in Use
```bash
# Find process using port
lsof -i :80  # or :5252, :9000

# Kill the process
kill -9 <PID>
```

### Issue: Containers Not Communicating
- Ensure all services are in the same docker-compose file
- Use service names (e.g., `back-end`, `front-end`) not `localhost`
- Check network: `docker network ls`

### Issue: Submodules on GitHub
```bash
# Remove .git folders from task directories
find . -name ".git" -type d -exec rm -rf {} +

# Add and push
git add .
git commit -m "Fix: Remove submodules"
git push
```

### Issue: Permission Denied in Docker
```bash
# Add user to docker group (Linux)
sudo usermod -aG docker $USER
newgrp docker
```

## Learning Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [CORS Explained](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

## Contributing

Feel free to fork this repository and submit pull requests for any improvements!

## License

This project is part of the Holberton School curriculum.

## Author

Angel D. Bayo Torres

Created as part of Holberton School's Docker and containerization curriculum.

## Acknowledgments

- Holberton School
- Docker community
- Nginx community
- Flask community

---

**Happy containerizing! 🚀** 
