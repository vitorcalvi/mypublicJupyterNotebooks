# mypublicJupyterNotebooks

A public collection of reproducible Jupyter notebooks focused on auditable mechanistic AI experiments, local-first LLM deployments, recursive latent reasoning, and production-ready model integrations.

## What's inside

- **Recursive Latent Reasoner (RLR)** — fail-closed, checksumed experiments with independently computed ground truth
- **Coconut / LFM2.5 production builds** — structured-output and tool-use workflows with KV-cache optimization
- **MiroFish + Graphiti + Neo4j** — local-first Colab setups with no external API dependencies
- **DSPark Swarms benchmarks** — swarm evaluation harnesses and API benchmarks
- **HRM product scenarios** — hierarchical reasoning applied to real product decisions
- **Bonsai27 Colab workflows** — ngrok tunneling, CUDA fixes, and prebuilt environment recipes

Every notebook is designed to run top-to-bottom in Colab or a local GPU runtime, with explicit failure modes, checkpointing, and reproducibility checks.

## Contributing

Contributions are welcome. Before opening a PR:

1. Open an issue describing the notebook, the experiment, or the bug.
2. Make sure the notebook runs cleanly in a fresh Colab runtime (Runtime → Restart runtime).
3. Keep changes minimal and focused — one notebook per PR is ideal.
4. Do not commit secrets, API keys, or private data.
5. If you add a notebook, include a short markdown title cell at the top explaining what it proves, what it does not prove, and how to run it.

### Style

- Prefer reproducibility over brevity.
- Log configs, seeds, and environment details in every run.
- Use checksums or deterministic baselines where possible.
- Label synthetic data clearly.

## License

MIT
