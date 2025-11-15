## Docker — Quick Notes

### What is Docker?
- Docker is a platform for building, packaging, and running applications in lightweight, portable containers.
- Containers bundle an application and its dependencies into a single unit that runs consistently across environments.

### Images vs Containers — short definition
- Image: a read-only template (built from a Dockerfile). Think of it as the binary/artifact you distribute.
- Container: a running (read-write) instance created from an image. It is the runtime environment where your app executes.

### Why use Docker?
- Consistency: same runtime on dev, CI, and production.
- Isolation: processes run with controlled resources and namespaces.
- Portability: images run anywhere a Docker runtime exists.
- Efficiency: containers share the host kernel and are lighter than full VMs.

## How Docker works (high-level)

1. Docker Engine (Docker daemon) runs on the host and manages images, containers, networking, volumes.
2. The Docker client (CLI) sends commands (e.g., `docker build`, `docker run`) to the daemon via a REST API.
3. Container runtimes (containerd + runc) are used under the hood to create and run containerized processes per OCI specs.

## Under the hood: key Linux technologies
- Namespaces: provide isolation of PID, network, mount, IPC, UTS, and user space so containers see private resources.
- cgroups (control groups): limit and account for resource usage (CPU, memory, I/O) of containers.
- Union/overlay filesystems (overlayfs): allow image layers to be stacked efficiently; writable container layer sits on top.

These kernel features let containers feel like separate machines while remaining lightweight.

## Docker images

- Built by following steps in a `Dockerfile` (instructions like FROM, RUN, COPY, CMD).
- Composed of ordered immutable layers; each instruction typically creates a new layer.
- Layers are cached and shared between images, which reduces storage and speeds builds.
- Images are versioned with tags (e.g., `nginx:1.25`, `myapp:latest`) and stored in registries (Docker Hub, private registries).

Image lifecycle summary:
- Build: `docker build -t myapp:1.0 .`
- List: `docker images`
- Save/export: `docker save` / `docker export` (different: save includes image metadata; export is filesystem only)
- Push/pull: `docker push` / `docker pull` to/from registries

Best practices for images:
- Keep images small: choose minimal base images and clean up package caches.
- Leverage layer caching: order Dockerfile commands from least to most frequently changing.
- Use multi-stage builds to avoid shipping build tools in the final image.

## Docker containers

- A container is an isolated runtime for one or more processes.
- Each container has a writable top layer; changes in that layer do not go back into the image.
- Containers are ephemeral by design: treat them as replaceable. Persist state with volumes or external services.

Container lifecycle summary:
- Create & run: `docker run -d --name web -p 8080:80 nginx:latest`
- List running containers: `docker ps`
- List all containers: `docker ps -a`
- Stop/Start/Remove: `docker stop`, `docker start`, `docker rm`
- Inspect: `docker logs`, `docker inspect`

Important container features:
- Networking: containers get virtual network interfaces and can be connected to Docker networks (bridge, host, overlay).
- Volumes: named volumes or bind mounts for persistent storage outside the container's writable layer.
- Ports: map container ports to host ports with `-p hostPort:containerPort`.

## Common Docker commands (PowerShell examples)

```powershell
# Build an image from Dockerfile in current directory
docker build -t myapp:1.0 .

# Run a container in detached mode, map port, name it
docker run -d --name myapp -p 8080:80 myapp:1.0

# List images and containers
docker images
docker ps -a

# Stop and remove container
docker stop myapp; docker rm myapp

# Remove image
docker rmi myapp:1.0

# Inspect logs
docker logs -f myapp

# Save and load images
docker save -o myapp.tar myapp:1.0
docker load -i myapp.tar

# Push to registry (assumes you're logged in)
docker tag myapp:1.0 myregistry.example.com/myapp:1.0
docker push myregistry.example.com/myapp:1.0
```

## Image layering and build cache (why it matters)
- Each Dockerfile instruction (usually) creates a new layer. Layers are cached.
- If an early layer doesn't change, Docker reuses it, speeding builds.
- Put stable steps (install system packages) earlier and copy application files later to avoid cache invalidation.


## Docker architecture (components)
- Docker CLI: user-facing command-line.
- Docker daemon (dockerd): manages images, containers, networks, volumes.
- containerd: a daemon to manage container lifecycle (pulling images, storage, namespaces).
- runc: low-level runtime that creates containers according to the OCI runtime spec.
- Registries: store and distribute images (Docker Hub, private registries, AWS ECR, GCR, etc.).

## When to use containers vs VMs
- Use containers for microservices, stateless apps, and fast, reproducible environments.
- Use VMs where full kernel/OS isolation is required, or when running untrusted workloads requiring strong isolation.

## Troubleshooting tips
- Can't reach container service: check `docker ps`, `docker logs`, `docker inspect` for port bindings and network settings.
- Image build slow: analyze Dockerfile order and layer cache usage; reduce image size.
- Container crashes on start: `docker logs` and `docker inspect` for exit code and health checks.

## Best Practices (short)
1. Build small images; prefer slim or distroless base images.
2. Use multi-stage builds for compiled languages.
3. Avoid storing secrets in images; use environment variables + secret stores.
4. Keep containers immutable and disposable; persist state outside containers.
5. Tag images with immutable version tags (avoid `latest` for production deployments).


-- end of notes --
