---
name: librechat-perlmutter-setup
description: "LibreChat runs via Docker on the Mac, pointed at a NERSC Perlmutter model server through an SSH tunnel"
metadata: 
  node_type: memory
  type: project
  originSessionId: 642c30b7-6eef-4299-b861-d266775505ed
---

LibreChat is installed at `~/claudeworking/LibreChat` and runs via Docker Desktop (Mac, `docker compose up -d`). It serves at http://localhost:3080. Config: `librechat.yaml` defines a custom endpoint "NERSC Perlmutter" (model `qwen`, served by sglang); the API key lives in `.env` as `EXTERNAL_API_KEY`. A `docker-compose.override.yml` bind-mounts `librechat.yaml` into the container.

The model server runs on a Perlmutter compute node reached via an SSH tunnel from the Mac: `ssh -i ~/.ssh/nersc -N -L 30000:<node>:30000 wbhimji@perlmutter.nersc.gov`. The tunnel binds loopback only, but on Docker Desktop for Mac the container reaches it via `http://host.docker.internal:30000/v1` — works without rebinding the tunnel to 0.0.0.0 (that trick is Mac-specific; would NOT work on Docker/Linux).

**Why:** Perlmutter jobs move nodes between runs (e.g. nid001133 → nid001040). **How to apply:** when the endpoint stops responding, the fix is almost always to restart the SSH tunnel pointing at the new node — LibreChat itself needs no changes (it always targets `localhost:30000` via the tunnel). Verify health with `curl -s -o /dev/null -w "%{http_code}" -H "Authorization: Bearer <key>" http://localhost:30000/v1/models` (expect 200).
