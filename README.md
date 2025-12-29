# docker-images

Repository for building and deploying Docker images with automated vulnerability scanning and registry push.

## CI/CD Pipeline

The repository uses GitHub Actions to automatically build, scan, and deploy Docker images.

### Deploy Workflow (`.github/workflows/deploy-docker.yaml`)

**Trigger**: 
- Manual trigger via `workflow_dispatch`

**Image Details**:
- Image name: `helm3`
- Registry: Docker Hub
- Authentication: Uses `DOCKER_USERNAME` and `DOCKER_PASSWORD` secrets

**Pipeline Stages**:

1. **Vulnerability Scan** (vulnerability-scan job)
   - Runs on: `ubuntu-latest`
   - Uses: Trivy by Aqua Security for scanning
   - Scans: `${{ DOCKER_USERNAME }}/helm3:latest`

2. **Build & Push** (deploy job)
   - Depends on: Successful vulnerability scan
   - Runs on: `ubuntu-latest`
   - Builds from: `./docker` directory
   - Pushes to: Docker Hub as `${{ DOCKER_USERNAME }}/helm3:latest`

### Docker Scout Commands

View a summary of image vulnerabilities and recommendations:
```
docker scout quickview
```

View vulnerabilities for local image:
```
docker scout cves local://helm3:v1
```