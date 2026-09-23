# I'm open-sourcing my Jupyter notebook lab — and I want you to contribute

Most ML notebooks live as disposable scripts or hidden research artifacts. I'm changing that by publishing a small but growing library of **reproducible, auditable, local-first** notebooks — and I'm looking for contributors.

## What this repo is

[my_public_notebooks](https://github.com/vitorcalvi/my_public_notebooks) is a collection of Jupyter notebooks built to run top-to-bottom in Colab or a local GPU runtime. The emphasis is not on winning leaderboards. It is on **mechanistic rigor, reproducibility, and production realism**.

Right now the collection covers:

- **Auditable Recursive Latent Reasoner (RLR)** — recurrent state-update experiments with independently computed ground truth, checksums, and held-out evaluation
- **Coconut / LFM2.5 production builds** — replacing verbose chain-of-thought with continuous thoughts, optimized with KV-cache, applied to real structured-decision workloads
- **MiroFish + Graphiti + Neo4j** — fully local Colab setup with zero external API dependencies
- **DSPark Swarms benchmarks** — evaluation harnesses and API benchmarks
- **HRM product scenarios** — hierarchical reasoning applied to real product decisions
- **Bonsai27 Colab workflows** — ngrok tunneling, CUDA fixes, and prebuilt environment recipes

Every notebook tries to answer three questions honestly:
1. What does this prove?
2. What does this *not* prove?
3. How do I run it without guessing?

## Why I'm opening it up

Research notebooks usually die in a single Google Drive folder or a private Kaggle kernel. That makes them hard to audit, hard to extend, and easy to misrepresent. By publishing them as a living repo, I want to:

- Make experiments auditable instead of promotional
- Build a shared baseline for local-first and on-device LLM work
- Document the failure modes that papers and READMEs usually skip

## How to contribute

You don't need to be a researcher. Useful contributions come in a lot of forms:

- **Fix a broken cell** — notebooks rot fast; a working cell is a gift
- **Add a baseline** — every experiment needs a fair comparison
- **Port to CPU / MPS** — GPU-only notebooks leave Mac and CPU users behind
- **Add a production wrapper** — inference wrappers with timeouts, logging, and error handling
- **Document a failure mode** — what broke, why, and what you tried
- **Write a new notebook** — if it is reproducible, honest about its limits, and runnable in Colab, it fits

Before you open a PR:

1. File an issue so we can avoid duplicate work.
2. Run the notebook from a fresh Colab runtime.
3. Keep changes focused — one notebook per PR works best.
4. Do not commit secrets or private data.
5. If you add a notebook, start with a markdown cell that says what it proves, what it does not prove, and how to run it.

Full contribution guidelines are in the [README](https://github.com/vitorcalvi/my_public_notebooks).

## The stack I care about right now

If you want an entry point, these are the areas where an extra pair of hands helps most:

- **LFM2.5 / Coconut** — production wrappers, latency benchmarks, on-device paths
- **Graphiti + Neo4j local pipelines** — alternative backends, memory persistence, evaluation harnesses
- **RLR baselines** — Mamba-2, GRU-RSSM, and transformer comparisons under the same evaluation protocol
- **Colab environment recipes** — pinned versions, install order, and known gotchas for T4 runtimes

## Let's make notebook research less disposable

If you have ever debugged a 12-hour Colab run only to find the artifact never got saved, or tried to reproduce a result from a paper with no code and no seed list, you already know why this matters.

The repo is public. The notebooks are runnable. The failures are documented.

[Star the repo](https://github.com/vitorcalvi/my_public_notebooks), open an issue, send a PR, or just run a notebook and tell me what broke.
