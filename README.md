# FastAPI Docker Deployment

## Project Overview
A FastAPI application dockerized with Docker Compose, monitored using Prometheus & Grafana, and deployed via Jenkins pipeline.

## Tech Stack
- FastAPI
- Docker & Docker Compose
- Prometheus & Grafana
- cAdvisor
- Jenkins

## Application Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | / | Returns a hello message |
| GET | /users | Returns list of users stored in JSON file |
| POST | /users | Accepts and stores user data in JSON file |

## Project Structure
docker-fastapi-test/
├── app/
│   ├── main.py
│   ├── services.py
│   └── schema.py
├── data/               ← auto created, stores users.json
├── Dockerfile
├── docker-compose.yml
├── prometheus.yml
├── Jenkinsfile
└── README.md
## Setup & Run

### Prerequisites
- Docker
- Docker Compose
- Jenkins
- Java 21

### Run the application
```bash
git clone https://github.com/ChetanKathane/Fastapi-app.git
cd docker-fastapi-test
docker-compose up --build -d
```

### Access the application
| Service | URL |
|---------|-----|
| FastAPI docs | http://your-ip:8000/docs |
| Prometheus | http://your-ip:9090 |
| Grafana | http://your-ip:3000 |
| Jenkins | http://your-ip:8080 |

## Data Persistence
Data is stored in `users.json` inside the `data/` directory.
Docker volume ensures data persists even after container is destroyed and recreated.

### Test persistence
```bash
# Add a user
curl -X POST http://<localhost/your-ip>:8000/users \
  -H "Content-Type: application/json" \
  -d '{"first_name": "Chetan", "last_name": "kathane", "age": 23}'

# Destroy and recreate containers
docker-compose down
docker-compose up -d

# Data should still be present
curl http://<localhost/your-ip>:8000/users
```

## Monitoring
- **cAdvisor** collects container metrics
- **Prometheus** scrapes metrics from cAdvisor every 15 seconds
- **Grafana** visualizes metrics using cAdvisor dashboard (ID: 14282)

### Grafana Setup
1. Open http://<localhost/your-ip>:3000
2. Login with admin/admin
3. Add Prometheus data source: http://prometheus:9090
4. Import dashboard ID: 14282

## Jenkins Pipeline
Pipeline stages:
1. **Clone Repo** - Clones the repository
2. **Build Docker Image** - Builds the Docker image
3. **Deploy** - Deploys using docker-compose
4. **Health Check** - Verifies the application is running

## Jenkins Setup
1. Install Jenkins
2. Add jenkins user to docker group:
```bash
sudo usermod -aG docker jenkins
```
3. Create pipeline job and point to Jenkinsfile in repo