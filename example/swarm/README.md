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

## Prerequisites

- Docker Swarm initialized (single-node or multi-node cluster)
- All nodes have access to the required Docker images

## Deployment

1. Navigate to this directory:
   ```bash
   cd example/swarm
   ```

2. Deploy the stack:
   ```bash
   docker stack deploy -c docker-compose.yml apisix-stack
   ```

3. Check the status of services:
   ```bash
   docker service ls
   ```

4. View logs for a specific service:
   ```bash
   docker service logs apisix-stack_apisix
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

## Scaling Services

To scale a service, use the following command:
```bash
docker service scale apisix-stack_apisix=3
```

## Removing the Stack

To remove the entire stack:
```bash
docker stack rm apisix-stack
```

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