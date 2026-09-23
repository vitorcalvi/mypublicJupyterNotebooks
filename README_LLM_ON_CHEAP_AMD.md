# LLM on Cheap AMD (Dokploy Raw Docker)

This configuration runs `llama.cpp` with Vulkan and DSpark speculative decoding on an AMD GPU through Dokploy’s **Raw** compose provider.

## Dokploy Raw Docker configuration

In the Dokploy service’s **General** tab:

1. Open **Deploy Settings**.
2. Under **Provider**, select **Raw**.
3. Paste the following content into **Compose File**.
4. Deploy the service.

```yaml
services:
  lfm25-dspark:
    image: ghcr.io/ggml-org/llama.cpp:server-vulkan
    restart: unless-stopped

    devices:
      - /dev/dri:/dev/dri

    volumes:
      - /home/vitor/models/lfm25:/models:ro

    ports:
      - "8088:8080"

    command:
      - -m
      - /models/LFM2.5-2.6B-Q4_K_M.gguf
      - -md
      - /models/LFM2.5-2.6B-DSpark-Q4_K_M.gguf
      - --spec-type
      - draft-dspark
      - --spec-draft-n-min
      - "0"
      - --spec-draft-n-max
      - "7"
      - --spec-draft-p-min
      - "0"
      - -ngl
      - "999"
      - -fa
      - "on"
      - -c
      - "8192"
      - -np
      - "1"
      - -b
      - "2048"
      - -ub
      - "512"
      - -ctk
      - f16
      - -ctv
      - f16
      - -ctkd
      - f16
      - -ctvd
      - f16
      - --host
      - 0.0.0.0
      - --port
      - "8080"
      - --metrics

    healthcheck:
      test:
        - CMD-SHELL
        - wget -qO- http://127.0.0.1:8080/health >/dev/null 2>&1 || exit 1
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 90s
```

## Required host paths

The Dokploy Docker host must contain both model files at these exact paths:

```text
/home/vitor/models/lfm25/LFM2.5-2.6B-Q4_K_M.gguf
/home/vitor/models/lfm25/LFM2.5-2.6B-DSpark-Q4_K_M.gguf
```

The compose file mounts that directory read-only inside the container as `/models`.

## Port mapping

The container listens on port `8080`; Dokploy publishes it on host port `8088`:

```text
host:8088 -> container:8080
```

The healthcheck is internal to the container and checks:

```text
http://127.0.0.1:8080/health
```

Metrics are available from the server’s `/metrics` endpoint on the container’s port `8080`, which is mapped to host port `8088`.

## AMD GPU device

The configuration passes the host’s Direct Rendering Infrastructure device into the container:

```yaml
devices:
  - /dev/dri:/dev/dri
```

This is the device mapping used by the Vulkan container configuration shown above.

## Dokploy deployment controls

After saving the Raw compose configuration, use Dokploy’s deployment controls as needed:

- **Deploy**: start or apply the service.
- **Stop**: stop the service.
- **Rebuild**: recreate the service using the compose configuration.
- **Open Terminal**: open a terminal for the deployed service.
- **Logs**: inspect startup and runtime output.
- **Monitoring**: inspect the service after deployment.

The compose file itself is the complete service configuration; no additional Dokploy-specific YAML fields are included here.
