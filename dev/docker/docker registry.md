# Docker Registry (With Analogies)

A Docker Registry is ==where you store and share Docker images==. Let me explain it through comparisons.

## The Core Idea: A Library of Images

**Without a Registry:** You build an image and keep it only on your computer

```bash
docker build -t myapp:1.0 .
# Image exists only on YOUR computer
# Your teammate can't use it
# If your computer crashes, it's gone
```

**The analogy:**
- GitHub but for docker

## Types of Registries

### 1. Docker Hub 

Docker Hub is the default, free registry anyone can use.

```bash
# Login to Docker Hub
docker login
# Username: your-docker-username
# Password: your-docker-password (or token)

# Push image to Docker Hub
docker tag myapp:1.0 yourusername/myapp:1.0
docker push yourusername/myapp:1.0

# Pull someone else's image
docker pull nginx:latest
# This pulls from Docker Hub (default)

# Run someone's public image
docker run -d nginx:latest
```


### 2. Private Registries = Exclusive Libraries

For company secrets and proprietary code:

```bash
# Login to private registry
docker login mycompany-registry.com
# Username: employee@company.com
# Password: company-secret

# Push to private registry
docker tag myapp:1.0 mycompany-registry.com/myapp:1.0
docker push mycompany-registry.com/myapp:1.0

# Pull from private registry
docker pull mycompany-registry.com/myapp:1.0

# Only employees with credentials can access
```

### 3. Self-Hosted Registry = Your Own Library

Run your own registry server:

```yaml
# docker-compose.yml for a private registry
version: '3.8'

services:
  registry:
    image: registry:2
    ports:
      - "5000:5000"
    volumes:
      - registry-data:/var/lib/registry
    # Now you have a private registry at localhost:5000

volumes:
  registry-data:
```

```bash
# Push to your own registry
docker tag myapp:1.0 localhost:5000/myapp:1.0
docker push localhost:5000/myapp:1.0

# Pull from your own registry
docker pull localhost:5000/myapp:1.0
```

## Popular Registries

- Docker Hub
- Amazon ECR (Elastic Container Reg) 
- Google Container Registry (GCR)
- Azure Container Registry (ACR) 
- GitLab Container Registry
- Self-Hosted (open source registry:2) 

## Image Naming = Library Cataloguing System

```
username/repository:tag
│        │            │
│        │            └─ Version/variant (like book edition)
│        └──────────────── Image name (like book title)
└───────────────────────── Author/publisher (like author name)
```

**Examples:**

```bash
# Docker Hub public images (nginx, postgres, etc.)
docker pull nginx:latest
# Full: docker.io/library/nginx:latest

# Your images on Docker Hub
docker pull yourusername/myapp:1.0
# Full: docker.io/yourusername/myapp:1.0

# Private registry
docker pull mycompany-registry.com/backend/api:2.1
# Full path with registry domain

# AWS ECR (Amazon)
docker pull 123456789.dkr.ecr.us-east-1.amazonaws.com/myapp:1.0

# Google Cloud
docker pull gcr.io/my-project/myapp:1.0
```

**Naming conventions:**

```bash
# Tagging images with multiple names
docker build -t myapp:1.0 .

# Tag with registry
docker tag myapp:1.0 yourusername/myapp:1.0
docker tag myapp:1.0 yourusername/myapp:latest

# Now you have multiple tags pointing to the same image
docker images | grep myapp
# Shows:
# yourusername/myapp    1.0        abc123...
# yourusername/myapp    latest     abc123...
```

## Useful Commands

```bash
# Authentication
docker login                    # Login to Docker Hub
docker login myregistry.com     # Login to custom registry
docker logout                   # Logout

# Tagging
docker tag myapp:1.0 username/myapp:1.0
docker images                   # Show all images and tags

# Pushing
docker push username/myapp:1.0
docker push username/myapp      # Pushes all tags

# Pulling
docker pull nginx:latest
docker pull myregistry.com/api:1.0

# Searching (Docker Hub only)
docker search nginx
# Shows available images

# Cleanup
docker image rm myapp:1.0       # Remove local image
docker image prune              # Remove unused images
docker system prune -a          # Remove everything unused

# Inspect registry
docker inspect username/myapp:1.0   # Local image info
# Use registry API for registry info
curl https://registry.hub.docker.com/v2/nginx/manifests/latest
```

## Key Takeaways

| Concept        | Purpose             | Analogy              |
| -------------- | ------------------- | -------------------- |
| **Registry**   | Store images        | Library              |
| **Repository** | Image family        | Book series          |
| **Tag**        | Image version       | Edition number       |
| **Layer**      | Image component     | Chapter in book      |
| **Push**       | Upload image        | Donate to library    |
| **Pull**       | Download image      | Borrow from library  |
| **Docker Hub** | Public registry     | Public library       |
| **Private**    | Restricted registry | Members-only library |
| **Auth**       | Access control      | Library card         |

## Quick Reference

```bash
# Build
docker build -t myapp:1.0 .

# Tag with registry
docker tag myapp:1.0 yourusername/myapp:1.0

# Login
docker login

# Push
docker push yourusername/myapp:1.0

# Others can now pull
docker pull yourusername/myapp:1.0

# Private registry
docker login mycompany-registry.com
docker tag myapp:1.0 mycompany-registry.com/internal/myapp:1.0
docker push mycompany-registry.com/internal/myapp:1.0
```

Docker Registries are the distribution system that makes containerization truly powerful—they transform your local images into globally accessible, shareable, version-controlled artifacts!****