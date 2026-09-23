# Jupyter Notebook on Cheap AMD GPU

Dokploy **Raw** Docker Compose configuration for a ROCm PyTorch container that starts JupyterLab with AMD GPU device access.

> This configuration intentionally starts JupyterLab with no token and no password. Do not expose port `8888` to the public internet without adding authentication and appropriate network controls.

## Dokploy Raw Docker

In Dokploy:

1. Create or open a Compose service.
2. In **General** → **Provider**, select **Raw**.
3. Paste the compose configuration below into **Compose File**.
4. Replace `/home/username` in both volume mounts with the actual absolute path on the Dokploy host.
5. Deploy the service.

## Compose configuration

```yaml
services:
  jupyter-optimized:
    container_name: jupyter-optimized
    image: rocm/pytorch:latest
    pull_policy: always
    restart: unless-stopped
    ipc: host
    shm_size: 8g
    cpuset: "0-15"
    ports:
      - "8888:8888"
    environment:
      - OMP_NUM_THREADS=8
      - OPENBLAS_NUM_THREADS=8
      - MKL_NUM_THREADS=8
      - NUMEXPR_NUM_THREADS=8
      - HIP_VISIBLE_DEVICES=0
      - HSA_OVERRIDE_GFX_VERSION=11.0.0
    volumes:
      # Replace /home/username with your host's actual absolute path
      - /home/username/notebooks:/workspace
      - /home/username/.cache/huggingface:/root/.cache/huggingface
    devices:
      - /dev/kfd:/dev/kfd
      - /dev/dri:/dev/dri
    group_add:
      - video
      - render
      # Alternatively, use host GIDs directly (e.g., "44", "109")
    cap_add:
      - SYS_PTRACE
    security_opt:
      - seccomp:unconfined
    entrypoint: /bin/bash
    command:
      - -lc
      - |
        set -euo pipefail

        python -m pip install --no-cache-dir --upgrade \
          pip \
          jupyterlab \
          ipykernel \
          transformers \
          trl \
          peft \
          datasets \
          accelerate \
          safetensors \
          sentencepiece \
          protobuf

        python -m ipykernel install \
          --sys-prefix \
          --name rocm-python \
          --display-name "Python 3 (ROCm)"

        exec jupyter lab \
          --ip=0.0.0.0 \
          --port=8888 \
          --no-browser \
          --allow-root \
          --ServerApp.root_dir=/workspace \
          --ServerApp.token="" \
          --ServerApp.password=""
```

## Required host changes

Replace these paths before deployment:

```yaml
- /home/username/notebooks:/workspace
- /home/username/.cache/huggingface:/root/.cache/huggingface
```

The first mount makes host notebooks available at `/workspace` in the container. The second mount persists the Hugging Face cache on the host.

For example, if the Dokploy host user directory is `/home/vitor`, the equivalent mappings are:

```yaml
- /home/vitor/notebooks:/workspace
- /home/vitor/.cache/huggingface:/root/.cache/huggingface
```

## Access

The configuration publishes JupyterLab’s container port `8888` on host port `8888`:

```text
http://<DOKPLOY-HOST-IP>:8888
```

The supplied command sets `--ServerApp.token=""` and `--ServerApp.password=""`, so JupyterLab is configured without an application-level login prompt.

## ROCm device access

The container receives these host devices:

```yaml
devices:
  - /dev/kfd:/dev/kfd
  - /dev/dri:/dev/dri
```

It also adds the `video` and `render` groups:

```yaml
group_add:
  - video
  - render
```

The service sets `HIP_VISIBLE_DEVICES=0` and `HSA_OVERRIDE_GFX_VERSION=11.0.0` in its environment. The configuration uses `rocm/pytorch:latest` and pulls that image on deployment according to `pull_policy: always`.

## Runtime setup

Every time the container starts, the command:

1. Upgrades `pip` and installs JupyterLab plus the listed Python packages.
2. Registers a kernel named `rocm-python`, displayed in JupyterLab as `Python 3 (ROCm)`.
3. Starts JupyterLab with `/workspace` as the server root directory.

The installed packages are:

```text
jupyterlab
ipykernel
transformers
trl
peft
datasets
accelerate
safetensors
sentencepiece
protobuf
```

## Resource settings

| Setting | Value in configuration |
|---|---:|
| CPU set | `0-15` |
| Shared memory | `8g` |
| OpenMP threads | `8` |
| OpenBLAS threads | `8` |
| MKL threads | `8` |
| NumExpr threads | `8` |
| Exposed host port | `8888` |

`ipc: host` is included alongside `shm_size: 8g`, as provided in the compose configuration.

## Security note

This setup grants the container GPU device access and uses `SYS_PTRACE` plus `seccomp:unconfined`. It also configures JupyterLab with an empty token and password. Treat it as a trusted-LAN or otherwise protected deployment, and restrict network exposure at the Dokploy host, firewall, reverse proxy, or VPN layer as appropriate.
