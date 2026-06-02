# ExcaliDocker 🎨🐳

A homemade, lightweight Docker Compose setup to easily spin up a local instance of [Excalidraw](https://github.com/excalidraw/excalidraw).

This project is perfect for self-hosting your own private drawing board on your local network or homelab without relying on external servers.


## Features

- **Quick Setup:** One command to start a fully functional Excalidraw instance.
- **Stable:** Uses a pinned Docker image hash to ensure your setup doesn't break with unexpected upstream changes.
- **Health Monitored:** Built-in Docker health checks to ensure the container is running and serving traffic properly.


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
👉 **[http://localhost:3030](http://localhost:3030)**


## Configuration Details

The `docker-compose.yaml` is pre-configured with the following settings:

* **Port Mapping:** Binds the host machine's port `3030` to the container's port `80`. If port `3030` is already in use on your machine, you can change it in the `docker-compose.yaml` file (e.g., `"8080:80"`).
* **Restart Policy:** Set to `unless-stopped`, meaning the container will automatically start up if your machine reboots or Docker restarts, unless you manually stop it.
* **Image Hash:** Uses a specific SHA256 image digest (`f7ee194...`) for reproducible deployments.


## Stopping the Instance

To stop the container, run the following command in the directory containing your `docker-compose.yaml`:

```bash
docker compose down
```


## Contributing

Feel free to submit issues or pull requests if you have suggestions for improvements or additional configurations


## License

This project is open-source. (Add your chosen license here, e.g., MIT)