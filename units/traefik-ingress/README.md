# Traefik-ingress

Starts in host mode and routes incoming http traffic to containers

## Annotate containers

Add labels to ingress HTTP on ${ingress_host} to port ${service_port}

```yaml
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.${app}.rule=Host(`${ingress_host}`)"
      - "traefik.http.routers.${app}.entrypoints=web"
      - "traefik.http.routers.${app}.service=${app}"
      - "traefik.http.services.${app}.loadbalancer.server.port=${service_port}"
```
