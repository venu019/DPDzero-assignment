# DevOps Reverse Proxy with Docker Compose

## 📌 Overview
This project demonstrates a simple microservices setup using **Docker Compose**, featuring:

- 🟦 **Service 1**: A Go-based HTTP server
- 🟨 **Service 2**: A Python Flask HTTP server
- 🔀 **Nginx**: A reverse proxy container routing to both services

Key Features:

- 🧭 URL path-based routing via Nginx (`/service1`, `/service2`)
- 📋 Access logging with timestamp and request path
- ✅ Health checks for both backend services
- 🔧 Full system startup with a single command

## 🗂️ Project Structure

project-root/
│
├── docker-compose.yml
├── service\_1/
│   ├── main.go
│   ├── go.mod
│   └── Dockerfile
├── service\_2/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
└── nginx/
├── nginx.conf
└── Dockerfile

## ⚙️ How It Works

| Component   | Description                                      | Port  |
|-------------|-------------------------------------------------|--------|
| `service1`  | Go web service (returns JSON)                   | 8001   |
| `service2`  | Python Flask service                            | 8002   |
| `nginx`     | Reverse proxy for both services                 | 8080   |

### 🧭 Nginx Routing

| URL Path Prefix      | Routed To                   |
|----------------------|-----------------------------|
| `/service1/`         | http://service1:8001/       |
| `/service2/`         | http://service2:8002/       |

## 🚀 Deployment Instructions

### ✅ Prerequisites

- [Docker](https://www.docker.com/products/docker-desktop) installed
- [Docker Compose](https://docs.docker.com/compose/install/) (if not included in Docker)

### 🔨 Build and Run All Services

In the root project directory, run:

```bash
docker-compose up --build
````

This will:

* Build the Go, Python, and Nginx images
* Start each container in the correct order
* Wait for backend services to pass health checks before starting Nginx

### 🌐 Access the Services via Nginx

* Service 1:

  * [http://localhost:8080/service1/ping](http://localhost:8080/service1/ping)
  * [http://localhost:8080/service1/hello](http://localhost:8080/service1/hello)

* Service 2:

  * [http://localhost:8080/service2/ping](http://localhost:8080/service2/ping)
  * [http://localhost:8080/service2/hello](http://localhost:8080/service2/hello)

## 🔍 Logging

Nginx logs every incoming request with:

* Timestamp
* Method and path
* Status code
* Upstream server info

To view logs:

```bash
docker logs nginx
```

## 🧪 Health Checks

Both services include Docker health checks:

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:<port>/ping"]
  interval: 10s
  timeout: 5s
  retries: 3

Nginx depends on both services being healthy before it starts.
```

## 🛑 Stopping Services

To stop all containers:

```bash
docker-compose down
```

## 🧹 Clean-Up

To rebuild cleanly (if things break):

```bash
docker-compose down -v --remove-orphans
docker-compose build --no-cache
docker-compose up
````

## 🔧 Setup Instructions

```bash
git clone <your_repo>
cd <your_repo>
docker-compose up --build
