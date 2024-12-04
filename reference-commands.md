# Docker Image Creation Command Reference Guide

### Project Content Table
- [Core Container Operations](#core-container-operations)
- [Image Management](#image-management)
- [Verification and Troubleshooting](#verification-and-troubleshooting)
- [Cleanup Operations](#cleanup-operations)

> **Author**: [Md Toriqul Islam](https://linkedin.com/in/thetoriqul)  
> **Description**: Comprehensive command reference for Docker image creation from containers  
> **Learning Focus**: Docker container manipulation and image management  
> **Note**: Review commands carefully before execution in production environments.

## Core Container Operations

### Step 1: Container Creation
```bash
# Create and enter a new container
docker run -it \
    --name hw_container \
    ubuntu:latest \
    /bin/bash

# Verification command
docker ps -a | grep hw_container
```

### Step 2: Container Modification
```bash
# Create test file (execute inside container)
touch HelloWorld.txt

# Exit container
exit

# Verify container status
docker container ls -a --filter name=hw_container
```

### Step 3: Image Creation
```bash
# Commit container changes to new image
docker container commit hw_container hw_image

# Verify image creation
docker images | grep hw_image
```

## Image Management

### Image Inspection
```bash
# View detailed image information
docker image inspect hw_image

# Check image history
docker history hw_image

# List all images
docker images --all
```

### Image Operations
```bash
# Tag image
docker tag hw_image hw_image:v1.0

# Save image to tar file
docker save hw_image > hw_image.tar

# Load image from tar file
docker load < hw_image.tar
```

## Verification and Troubleshooting

### Container Verification
```bash
# Create verification container
docker run -it \
    --name verify_container \
    hw_image \
    /bin/bash

# Check file existence
ls -la HelloWorld.txt

# View container logs
docker logs hw_container
```

### System Information
```bash
# Check Docker system info
docker system info

# View disk usage
docker system df

# Monitor container resources
docker stats verify_container
```

## Cleanup Operations

### Container Cleanup
```bash
# Stop all project containers
docker stop hw_container verify_container

# Remove all project containers
docker rm -f $(docker ps -aq --filter name=hw)

# Clean container volumes
docker container prune -f
```

### Image Cleanup
```bash
# Remove specific image
docker rmi hw_image

# Remove all unused images
docker image prune -a

# Complete system cleanup
docker system prune -af
```

## Learning Notes

1. Container changes are stored in writable layers
2. Image commits create new layers based on modifications
3. Each command creates a new filesystem layer
4. Tags help in version management
5. Regular cleanup prevents resource exhaustion

---

> 💡 **Best Practice**: Always tag images with meaningful versions

> ⚠️ **Warning**: Container commits increase image size with each layer

> 📝 **Note**: Use `docker history` to understand image composition