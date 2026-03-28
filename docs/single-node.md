# Single Node Setup

This repository already packages a Docker Compose stack for one Garage S3 node plus the community web UI. Follow these steps to stand up a minimal deployment inside your LXC.

## 1. Prepare the host

1. Install Docker and Docker Compose in the container.
2. Clone this repo and `cd` into the checkout root (`garage-s3`).
3. Create the directories Garage expects:
   ```bash
   mkdir -p meta data
   ```

## 2. Configure Garage

1. Copy `garage.example.toml` to `garage.toml` and edit:
   ```bash
   cp garage.example.toml garage.toml
   ```
2. Populate the following values:
   * `rpc_secret` – a strong secret used internally by the node.
   * `admin.admin_token` – the token the web UI and CLI use.
   * Review `metadata_dir`/`data_dir` if you want to store objects somewhere else.
3. Keep your secrets out of git with an env file next to the compose file:
   ```ini
   GARAGE_ADMIN_TOKEN=verysecret
   GARAGE_CONFIG_FILE=garage.toml
   GARAGE_VERSION=latest
   GARAGE_WEBUI_VERSION=latest
   ```

## 3. Launch the node

Start the services:

```bash
docker compose up -d
```

The Compose stack exposes:

* S3 API: `http://<node-ip>:3900`
* Admin API: `http://<node-ip>:3903`
* Web UI: `http://<node-ip>:3909` (it talks to the admin API via the internal network)

## 4. Verify and exercise

* `docker compose ps` shows the `garage` and `garage-webui` containers.
* `docker compose logs -f garage` reveals runtime info if something fails.
* Point an S3 client to the node IP/port 3900 and use the admin token to manage buckets, or use the web UI.

## 5. Update or reconfigure

1. Stop the stack:
   ```bash
   docker compose down
   ```
2. Edit `garage.toml` or `.env` as needed.
3. Restart:
   ```bash
   docker compose up -d
   ```

With this single-node stack you now have Garage S3 running on your homelab host. Expand later by adding nodes, replicating data, and adjusting the TOML to match your desired topology.
