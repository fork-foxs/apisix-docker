# Apache APISIX Docker Swarm Configuration

This directory contains Docker Swarm compatible configuration files for Apache APISIX and its related services.

## Overview

The Docker Swarm configuration includes:
- Apache APISIX (with 2 replicas for high availability)
- etcd (key-value store)
- Two upstream web services (web1 and web2)
- Prometheus (metrics collection)
- Grafana (metrics visualization)
- APISIX Dashboard (management UI)

## Key Differences from Regular Docker Compose

1. **Network**: Uses overlay network for multi-host communication
2. **Deployment Configuration**: Each service has deployment constraints and resource limits
3. **Replicas**: APISIX is configured with 2 replicas for high availability
4. **Placement**: Services are constrained to run on worker nodes
5. **Restart Policies**: Configured for production environment with failure conditions
6. **Configuration Management**: Uses Docker configs instead of bind mounts for better portability

## Prerequisites

- Docker Swarm initialized (single-node or multi-node cluster)
- All nodes have access to required Docker images

## Deployment

This configuration uses Docker configs which is the proper way to handle configuration files in Docker Swarm. It's portable and doesn't require absolute paths.

1. Navigate to this directory:
   ```bash
   cd example/swarm
   ```

2. Deploy the stack:
   ```bash
   docker stack deploy -c docker-compose.yml apisix-stack
   ```

## For Different Server Paths

When deploying on a different server, simply copy the entire `swarm` directory to your server and deploy with `docker-compose.yml`. No path changes are needed since Docker configs are used instead of bind mounts.

## Configuration Updates

If you need to update configuration files after deployment:

1. Modify the local configuration files
2. Update the Docker configs:
   ```bash
   # Example for APISIX config
   docker config rm apisix-stack_apisix_config
   docker config create apisix-stack_apisix_config ./apisix_conf/config.yaml
   ```
3. Update the service to use the new config:
   ```bash
   docker service update --config-add apisix-stack_apisix_config apisix-stack_apisix
   ```

## Managing the Stack

1. Check the status of services:
   ```bash
   docker service ls
   ```

2. View logs for a specific service:
   ```bash
   docker service logs apisix-stack_apisix
   ```

3. Scale a service:
   ```bash
   docker service scale apisix-stack_apisix=3
   ```

4. Remove the entire stack:
   ```bash
   docker stack rm apisix-stack
   ```

## Accessing Services

Once deployed, you can access the services at:

- **APISIX API Gateway**: http://localhost:9080
- **APISIX Admin API**: http://localhost:9180
- **APISIX Dashboard**: http://localhost:9000
- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3002
- **Web1**: http://localhost:9081
- **Web2**: http://localhost:9082

## Configuration Files

All configuration files are located in their respective subdirectories:
- `apisix_conf/config.yaml` - APISIX configuration
- `dashboard_conf/config.yaml` - Dashboard configuration
- `prometheus_conf/prometheus.yml` - Prometheus configuration
- `grafana_conf/` - Grafana configuration, dashboards, and provisioning
- `upstream/` - Nginx configurations for upstream services

## Notes

- The etcd data is persisted in a Docker volume named `etcd_data`
- All services are configured to restart on failure
- Resource limits are set to ensure fair resource usage across services
- The overlay network allows services to communicate across multiple Docker hosts
- Docker configs are used for configuration files, making the deployment portable across different environments
- Configuration changes require updating the Docker configs and redeploying the affected services