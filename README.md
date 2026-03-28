## Garage S3 homelab backend

This repo exposes a single Garage S3 node via Docker Compose plus a community web UI. Follow the MkDocs docs (`docs/index.md` and `docs/single-node.md`) for setup steps.

### Quick start

1. Copy `garage.example.toml` to `garage.toml` and set `rpc_secret` + `admin.admin_token`.
2. Populate `.env` with `GARAGE_ADMIN_TOKEN` and optional versions/config overrides.
3. Create `meta/` and `data/` directories, then run:
   ```bash
   docker compose up -d
   ```

### Serving docs

```bash
pip install mkdocs
mkdocs serve
```
