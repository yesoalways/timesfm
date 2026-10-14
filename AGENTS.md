# TimesFM — Agent Entry Point

This repository ships a first-party **Agent Skill** for TimesFM at:

```
timesfm-forecasting/
└── SKILL.md    ← read this for the full skill
```

## Install the skill

Copy the skill directory into your agent's skills folder:

```bash
# Cursor / Claude Code / OpenCode / Codex (global install)
cp -r timesfm-forecasting/ ~/.cursor/skills/
cp -r timesfm-forecasting/ ~/.claude/skills/

# Or project-level
cp -r timesfm-forecasting/ .cursor/skills/
```

Any agent that supports the open [Agent Skills standard](https://agentskills.io) will discover it automatically.

## Working in this repo

If you are developing TimesFM itself (not using it), the source lives in `src/timesfm/`.
Archived v1/v2 code and notebooks are in `v1/`.

Current tests and the CI package build:

```bash
.venv/bin/python -m pytest tests/
.venv/bin/python -m build
```

See `README.md` for full developer setup.

## Cloud Agent

This image has no GPU. The environment install creates `.venv` here with CPU PyTorch (`torch` from the CPU wheel index) and CPU JAX. The `flax` extra in `pyproject.toml` depends on `jax[cuda]`, which does not run on this image; CPU JAX is installed instead so `tests/test_model_loading.py` can import the Flax wrapper.

Load the pretrained checkpoint with `torch_compile=False`. The default `torch_compile=True` calls `torch.compile`, which is the wrong path on CPU. The sibling checkouts `Claude-Code-Game-Studios` and `desktop-tutorial` have no install or runtime.
