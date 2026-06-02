# ExcaliDocker 🎨🐳

A homemade, lightweight Docker Compose setup to easily spin up a local instance of [Excalidraw](https://github.com/excalidraw/excalidraw).

This project is perfect for self-hosting your own private drawing board on your local network or homelab without relying on external servers.


## Features

- **Quick Setup:** One command to start a fully functional Excalidraw instance.
- **Stable:** Uses a pinned Docker image hash to ensure your setup doesn't break with unexpected upstream changes.
- **Health Monitored:** Built-in Docker health checks to ensure the container is running and serving traffic properly.
- **Hardened:** Runs with a read-only filesystem, dropped Linux capabilities, and resource limits for a locked-down, least-privilege container.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/) (Included in Docker Desktop)


## Getting Started

1. **Clone the repository**:
```bash
git clone https://github.com/01xR4in/ExcaliDocker.git
cd ExcaliDocker
```


2. **Start the Excalidraw container:**
```bash
docker compose up -d
```
*The `-d` flag runs the container in the background (detached mode).*


3. **Access Excalidraw:**
Open your favorite web browser and navigate to:
👉 **[http://localhost:3030](http://localhost:3030)** *(or whatever you set `HOST_PORT` to)*

## Configuration Details

The `docker-compose.yaml` is pre-configured with the following settings:

* **Port Mapping:** The host port is set via the `HOST_PORT` variable in `.env` (defaults to `3030`) and maps to the container's port `80`. See [Configuring the port](#configuring-the-port) below.
* **Restart Policy:** Set to `unless-stopped`, meaning the container will automatically start up if your machine reboots or Docker restarts, unless you manually stop it.
* **Image Hash:** Uses a specific SHA256 image digest (`f7ee194...`) for reproducible deployments.


## Security Hardening

This setup runs Excalidraw with a locked-down container configuration following least-privilege principles:

- **Read-only root filesystem** (`read_only: true`) — the container can't write anywhere on disk. The few paths nginx needs for scratch space (`/var/cache/nginx`, `/var/run`, `/tmp`) are mounted as RAM-backed `tmpfs`, which is wiped on stop.
- **Dropped Linux capabilities** (`cap_drop: ALL`) — all kernel capabilities are removed, then only the four nginx actually needs at startup are added back:
  - `CHOWN` — set ownership of the temp dirs to the worker user
  - `SETUID` / `SETGID` — drop worker processes from root to the unprivileged `nginx` user
  - `NET_BIND_SERVICE` — bind to port 80
- **No privilege escalation** (`no-new-privileges:true`) — the process and its children can never gain more privileges than they start with (neutralizes setuid binaries).
- **Resource limits** — capped at 256 MB RAM and 1 CPU so a runaway process can't starve the host.


### Configuring the port

The host port is read from a `.env` file, defaulting to `3030` if unset. Copy the example and edit as needed:

```bash
cp .env.example .env
# then set HOST_PORT=8080 (or whatever you like)
```

`docker compose config` will show the resolved port before you start.


## Stopping the Instance

To stop the container, run the following command in the directory containing your `docker-compose.yaml`:

```bash
docker compose down
```


## Contributing

Feel free to submit issues or pull requests if you have suggestions for improvements or additional configurations


## License

This project is licensed under the [MIT License](LICENSE).