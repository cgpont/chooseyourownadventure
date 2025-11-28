# Container Management Rules

- **No Direct Container Changes**: Never modify anything directly inside Docker containers
- **Infrastructure as Code**: All changes must be done in Dockerfile, docker-compose.yml, or source code
- **Reproducible Builds**: Containers must be rebuildable from source files at any time
- **Version Control**: All configuration changes must be committed to the repository