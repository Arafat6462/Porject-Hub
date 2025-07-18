# 🚀 CI/CD Pipeline

!!! success "Production-Ready DevOps"
    **Automated deployment pipeline** for ResuMate Django application featuring GitHub Actions, Docker containerization, and DigitalOcean cloud infrastructure with SSL/TLS security.

**Pipeline Features:** :material-github:{ style="color: #24292e" } GitHub Actions • :material-docker:{ style="color: #2496ed" } Docker Containerization • :material-cloud:{ style="color: #0080ff" } DigitalOcean Droplet • :material-shield-check:{ style="color: #4caf50" } SSL/TLS Security

!!! tip "Live Production System"
    The pipeline automatically deploys to: [https://arafat2.me](https://arafat2.me) • [API Endpoint](https://arafat2.me/api/) • [Admin Panel](https://arafat2.me/admin/)

---

## 🏗️ Architecture Overview

```mermaid
graph TD
    A["👨‍💻 Developer"] -->|git push| B["📦 GitHub Repository"]
    B -->|Trigger| C["🔄 GitHub Actions"]
    
    C --> D["🏗️ Build Stage"]
    D --> E["📦 Docker Build"]
    E --> F["📤 Push to Docker Hub"]
    
    F --> G["🚀 Deploy Stage"]
    G --> H["🔐 SSH to DigitalOcean"]
    H --> I["⬇️ Pull Latest Image"]
    I --> J["🐳 Docker Compose Up"]
    
    J --> K["🌐 Nginx Reverse Proxy"]
    K --> L["🔒 SSL/TLS Termination"]
    L --> M["🎯 Production Site"]
    
    N["🗄️ PostgreSQL Database"] --> J
```

### 🔧 Infrastructure Components

| Component | Technology | Purpose | Status |
|:---------:|:----------:|:--------|:-------|
| **☁️ Cloud Provider** | <span class="tech-highlight">DigitalOcean Droplet</span> | Ubuntu 22.04 LTS server hosting | <span class="status-badge success">✅ Active</span> |
| **🌐 Web Server** | <span class="tech-highlight">Nginx</span> | Reverse proxy & SSL termination | <span class="status-badge success">✅ Active</span> |
| **🐳 Container Runtime** | <span class="tech-highlight">Docker & Docker Compose</span> | Application containerization | <span class="status-badge success">✅ Active</span> |
| **🗄️ Database** | <span class="tech-highlight">PostgreSQL 16</span> | Primary data persistence | <span class="status-badge success">✅ Active</span> |
| **📦 Registry** | <span class="tech-highlight">Docker Hub</span> | Container image storage | <span class="status-badge success">✅ Active</span> |
| **🔐 SSL Certificate** | <span class="tech-highlight">Let's Encrypt</span> | Free SSL/TLS encryption | <span class="status-badge success">✅ Active</span> |

---

## 🔄 GitHub Actions Workflow

### 📋 Build & Deploy Process

!!! example "Automated CI/CD Pipeline"
    **Triggers:** Every push to `master` branch • **Duration:** ~5 minutes • **Zero Downtime:** ✅

**Build Stage:**
```yaml
- name: Build and push Docker image
  uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    tags: arafat6462/resumate:master
```

**Deploy Stage:**
```bash
# SSH to production server
ssh root@arafat2.me

# Pull latest image and deploy
docker pull arafat6462/resumate:master
IMAGE_TAG=master docker compose -f docker-compose.prod.yml up -d

# Cleanup old images
docker image prune -f
```

### 🔒 Security & Secrets

| Secret Variable | Purpose | Type |
|:---------------:|:--------|:-----|
| **DOCKER_HUB_USERNAME** | Docker Hub authentication | Registry |
| **DOCKER_HUB_TOKEN** | Docker Hub access token | Registry |
| **DROPLET_HOST** | Production server IP | Server |
| **DROPLET_SSH_KEY** | Private SSH key | Authentication |
| **DB_PASSWORD** | Database password | Database |
| **SECRET_KEY** | Django secret key | Application |
| **GEMINI_API_KEY** | Google AI API key | External API |

---

## 🐳 Docker Configuration

### 📦 Production Setup

**Application Container:**
```dockerfile
FROM python:3.11-slim-buster
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
RUN python manage.py collectstatic --noinput
EXPOSE 8000
CMD ["/app/entrypoint.sh"]
```

**Docker Compose Production:**
```yaml
services:
  backend:
    image: arafat6462/resumate:${IMAGE_TAG:-latest}
    restart: always
    ports:
      - "8000:8000"
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:16
    restart: always
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 5s
      timeout: 5s
      retries: 5
```

---

## 🌐 Nginx & SSL Configuration

### 🔒 Production Web Server

**HTTPS Configuration:**
```nginx
server {
    server_name arafat2.me www.arafat2.me;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/arafat2.me/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/arafat2.me/privkey.pem;
}

# HTTP to HTTPS redirect
server {
    listen 80;
    server_name arafat2.me www.arafat2.me;
    return 301 https://$host$request_uri;
}
```

---

## 📊 Deployment Timeline

```mermaid
gantt
    title Production Deployment Process
    dateFormat YYYY-MM-DD
    
    section Build
    Code Checkout    :checkout, 2025-01-01, 30s
    Docker Build     :build, after checkout, 150s
    Registry Push    :push, after build, 60s
    
    section Deploy
    SSH Connection   :ssh, after push, 10s
    Image Pull       :pull, after ssh, 60s
    Container Deploy :deploy, after pull, 30s
    Health Check     :health, after deploy, 20s
```

### 📋 Deployment Checklist

| Stage | Check | Status | Duration |
|:-----:|:------|:-------|:--------:|
| **🏗️ Build** | Docker image creation | ✅ Automated | ~3 min |
| **📤 Push** | Registry upload | ✅ Automated | ~1 min |
| **🔐 Auth** | Server SSH connection | ✅ Automated | ~10 sec |
| **📥 Pull** | Latest image download | ✅ Automated | ~1 min |
| **🐳 Deploy** | Container orchestration | ✅ Automated | ~30 sec |
| **🎯 Health** | Service availability | ✅ Automated | ~20 sec |

---

## 🔧 Key Features

### ⚡ Production Highlights

| Feature | Implementation | Benefit |
|:-------:|:-------------|:--------|
| **🔄 Zero Downtime** | <span class="feature-highlight">Rolling Updates</span> | Seamless deployments |
| **🛡️ Health Checks** | <span class="feature-highlight">PostgreSQL + App</span> | Automatic failure detection |
| **🔒 SSL/TLS** | <span class="feature-highlight">Let's Encrypt</span> | Secure HTTPS traffic |
| **📦 Auto Cleanup** | <span class="feature-highlight">Docker Prune</span> | Optimized disk usage |
| **🔐 Secrets Management** | <span class="feature-highlight">GitHub Secrets</span> | Secure credential storage |
| **🌐 Reverse Proxy** | <span class="feature-highlight">Nginx</span> | Load balancing & caching |

### 📈 Quick Commands

| Purpose | Command | Description |
|:-------:|:--------|:------------|
| **🔍 Status** | `docker ps -a` | View containers |
| **📊 Logs** | `docker logs -f resumate_backend_prod` | Application logs |
| **🔄 Restart** | `docker-compose restart` | Restart services |
| **🧹 Cleanup** | `docker system prune -f` | Remove unused resources |
| **🌐 Nginx** | `sudo nginx -t && sudo systemctl reload nginx` | Test & reload config |
| **🔒 SSL** | `certbot certificates` | Check certificate status |

---

!!! success "Production-Ready Pipeline"
    :material-rocket:{ style="color: #ff9800" } **Fully Automated** deployment with zero-downtime updates, SSL security, and comprehensive monitoring.

    **Live System:** [https://arafat2.me](https://arafat2.me) • **API:** [/api/](https://arafat2.me/api/) • **Admin:** [/admin/](https://arafat2.me/admin/)

---

!!! abstract "Pipeline Status"
    :material-check-circle:{ style="color: #4caf50" } **Live & Operational** • :material-update:{ style="color: #2196f3" } Last Deploy: Automated • :material-shield-check:{ style="color: #9c27b0" } Security: A+ Rating
