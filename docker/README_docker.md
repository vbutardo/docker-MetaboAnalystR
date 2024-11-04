# Docker Implementation for MetaboAnalystR

This directory contains the Docker configuration for MetaboAnalystR with Jupyter integration. The implementation provides a reproducible environment for metabolomics analysis.

## Directory Contents
- `Dockerfile`: Container configuration and build instructions
- `README_docker.md`: Docker implementation documentation

## Container Specifications

### Base Image and Core Components
- Base: `continuumio/miniconda3:latest`
- R: 4.3.3
- Python: 3.9
- Bioconductor: 3.18
- Jupyter Lab: Latest

### Container Structure
```
/app/
├── data/               # Data directory
│   ├── processed/     # For processed files
│   └── temp/         # Temporary files
├── notebooks/         # Jupyter notebooks
├── results/          # Analysis results
├── environment/      # Environment files
└── work/            # Mount point for user files
```

### Resource Requirements
- Minimum RAM: 8GB (12GB recommended)
- Disk Space: 20GB
- CPU: 2+ cores recommended
- Docker Version: 20.10+

## Building the Container

### From Project Root
```bash
# Standard build
docker build -f docker/Dockerfile -t docker-metaboanalystr .

# Build with specific tag
docker build -f docker/Dockerfile -t docker-metaboanalystr:1.0 .
```

### From Docker Directory
```bash
cd docker
docker build -t docker-metaboanalystr .
```

## Running the Container

### Basic Usage
```bash
docker run -d \
  --name metaboanalystr \
  -p 8888:8888 \
  -v $(pwd):/app/work \
  docker-metaboanalystr
```

### With Resource Limits
```bash
docker run -d \
  --name metaboanalystr \
  -p 8888:8888 \
  -v $(pwd):/app/work \
  -m 12g \
  --cpus=2 \
  docker-metaboanalystr
```

### Development Mode
```bash
docker run -d \
  --name metaboanalystr-dev \
  -p 8888:8888 \
  -v $(pwd):/app/work \
  -v ${HOME}/.gitconfig:/home/analyst/.gitconfig \
  docker-metaboanalystr
```

## Security Features

### User Management
- Non-root user 'analyst'
- Limited permissions
- Secure volume mounts

### Configuration
- No default passwords
- Volume isolation
- Resource limits
- Health checks

## Container Components

### Pre-installed Software
- R and BioConductor packages
- Python scientific stack
- Git integration
- System libraries

### Jupyter Extensions
- Git integration
- Resource usage monitor
- Notebook diffing
- R kernel support

## Health Checks and Monitoring

### Automated Checks
- Jupyter server availability
- R environment validation
- Package installation verification
- Resource monitoring

### Log Locations
- Jupyter: `/app/notebooks/logs`
- R: `/app/logs`
- Container: Docker logs

## Data Management

### Volumes
- `/app/work`: User data (mounted)
- `/app/data`: Container data
- `/app/results`: Analysis outputs

### Persistence
- Use volume mounts for data
- Regular backup recommended
- Temporary file cleanup

## Common Operations

### Container Management
```bash
# Start container
docker start metaboanalystr

# Stop container
docker stop metaboanalystr

# View logs
docker logs metaboanalystr

# Access shell
docker exec -it metaboanalystr bash
```

### Data Management
```bash
# Copy files to container
docker cp data.csv metaboanalystr:/app/work/

# Copy from container
docker cp metaboanalystr:/app/results/analysis.pdf .

# Fix permissions
docker exec metaboanalystr chown -R analyst:analyst /app/work
```

## Troubleshooting

### Common Issues

1. Memory Errors
```bash
# Increase memory allocation
docker run -d --name metaboanalystr -m 12g docker-metaboanalystr
```

2. Permission Issues
```bash
# Fix ownership
docker exec metaboanalystr chown -R analyst:analyst /app/work
```

3. Port Conflicts
```bash
# Use different port
docker run -d -p 8889:8888 docker-metaboanalystr
```

### Validation

Run these commands to verify installation:
```bash
# Check R environment
docker exec metaboanalystr R -e "library(MetaboAnalystR)"

# Verify Jupyter
curl http://localhost:8888/api/status

# Run validation script
docker exec metaboanalystr Rscript /app/tests/validate_environment.R
```

## Development Notes

### Build Optimization
- Multi-stage builds available
- Cache intermediate layers
- Optimize layer ordering

### Testing
- Automated validation
- Integration tests
- Resource monitoring

### Updates
- Regular base image updates
- Package version management
- Security patches

## Support

For issues:
1. Check container logs
2. Verify resource availability
3. Review validation outputs
4. Submit issue with details:
   - Docker version
   - Host specifications
   - Error messages
   - Steps to reproduce