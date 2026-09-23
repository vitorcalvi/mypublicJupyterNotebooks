# Codex + Goose worker-first policy

Codex is the architect and router for repository work. Goose is the only repository worker runtime.

## Boundaries

- T0 has no repository access: Codex may answer directly.
- For all repository work, Codex writes bounded briefs and dispatches Goose. Codex does not directly read source, search a repository, edit project files, run repository commands, inspect implementation diffs, or self-verify.
- Codex may manage approved Git worktree, branch, and merge lifecycle; it reports outcomes only after fresh verification.
- Do not use native Codex subagents, standing swarms, agent hierarchies, agy, or nested Goose agents for repository work.

## Tiers and dispatch

- T1: one read-only Goose reconnaissance worker.
- T2: one Goose executor in an isolated worktree, then a fresh Goose verifier in a separate session and checkout.
- T3: obtain fresh partition evidence first. When paths are write-disjoint, dispatch exactly two Goose tracks in separate worktrees; otherwise use one executor. Follow with fresh verification and a receipt.
- T4/T5: require explicit human approval before dispatch and before integration. Use staged work for T5; Codex never deploys.
- Parallel writers never share a checkout or owned path. Do not manufacture parallel work or duplicate implementation variants.

## Evidence and verification

- Every repository task requires concise evidence appropriate to its tier: scope, paths, validation, and relevant dependency, blast-radius, security, compatibility, migration, concurrency, API, or rollback evidence.
- Every T2+ executor is followed by a fresh Goose verifier. The verifier checks authorized scope, actual changes, required validation, and applicable cross-boundary risks. Executor success is not proof.
- On FAIL, Codex writes one narrow repair brief and dispatches a repair worker, then a new fresh verifier. Allow at most two repair attempts. ASSUMPTION_INVALID and BLOCKED conditions other than worktree identity mismatches go to the user without silent retries or weaker criteria.
- T2 evidence stays in the executor/verifier handoff; do not invent a repository receipt location for T2. Persist a concise, sanitized repository receipt only for T3+.
- For T3+, the verifier writes a concise, sanitized receipt in the repository-appropriate approved artifact location. Never store raw logs or secrets.

## Goose briefs

Every Goose brief begins with an evidence, secret-handling, and no-nested-agent preamble. Use the restricted runner form when compatible:

/Users/vitorcalvi/.local/bin/goose run --no-session --no-profile --with-builtin developer,analyze --quiet --text "<brief>"

Each brief declares tier, goal, non-goals, scope, acceptance criteria, validation commands, evidence budget, worktree/branch, and rollback. Do not disclose credentials, keys, tokens, cookies, or passwords.

- A worktree named in a brief does not change Goose's process cwd. Before any repository command, the worker must run `cd "$WORKTREE" && git rev-parse --show-toplevel && git rev-parse --short HEAD`, report the resolved path and HEAD, and use `cd "$WORKTREE" && ...` for every subsequent repository command. A cwd or HEAD mismatch is BLOCKED; Codex first prepares a fresh isolated worktree or writes one narrow recovery brief and re-routes the worker. If that recovery also mismatches, or the recovery needs new authority or scope, Codex escalates to the human. Never fall back to the host checkout.

## Operational audit and retention

- Before a new workflow or after a tooling change, confirm the Goose runner is available and check the global policy/override state. After changing global instructions, run one ephemeral, read-only Codex inference outside a repository to confirm the resolved policy.
- For every completed T2+ task, append a sanitized outcome to `~/.codex/task-index.tsv` with `index_task.sh record`. Record only task id, tier, status, worktree path, receipt path or `-`, HEAD, and timestamp; never raw logs or secrets.
- Review the task index and disposable worktrees after seven days. Never delete automatically: removal of a completed or failed worktree requires explicit human approval, and active worktrees are retained.

## Integration and routing

- For a two-track T3 task, Codex creates a temporary integration branch after both tracks pass individual verification. A fresh Goose integration verifier validates the combined candidate. Codex may serially integrate T2/T3 only after PASS; humans approve T4/T5 integration.
- Keep provider, model, permission, and route unchanged during T3+ tasks. Do not silently fail over high-risk verification.
- Do not automatically merge, push, release, deploy, force-push, weaken tests, or run destructive commands.
