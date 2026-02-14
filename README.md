# jupyter-notebooks-skill

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)

Collaborate on Jupyter notebooks with AI agents without the usual notebook chaos. This skill gives your agent reliable, repeatable notebook workflows, so you spend less time fixing broken executions, noisy diffs, and manual cleanup, and more time doing analysis.

Full-lifecycle agent skill for Jupyter notebooks — 9 CLI tools built on `nbformat`, `nbclient`, `nbconvert`, `nbdime`, and `papermill`. Covers environment preflight checks, creation, cell-level editing, selective execution, parameterised batch runs, structural validation, format conversion, output management, and content-aware diffing and merging. Works with Claude Code, Codex, Cursor, and any agent that supports the skills spec.

## Capabilities

- **Selective range execution** — run cells 3–7 while preserving state from earlier cells, without re-executing the entire notebook
- **Parameterised batch runs** — papermill integration for templated execution across parameter sets
- **Structural validation and linting** — detect duplicate cell IDs, stale execution state, empty cells, forbidden outputs; semantic exit codes for CI pipelines and pre-commit hooks
- **Content-aware diff and merge** — cell-level diffs via `nbdime` (text or structured JSON), three-way merge with conflict detection, `nbstripout` integration for repository hygiene
- **Output management** — list outputs by cell, estimate sizes, extract embedded images to files, strip selectively or in bulk, clear execution counts
- **Cell operations** — 13 subcommands: add, update, delete, move, bulk insert, range delete, regex search, per-cell metadata and tag management
- **Format conversion** — export to HTML, PDF, LaTeX, Markdown, reStructuredText, slides, and scripts via `nbconvert`
- **Metadata management** — notebook-level and cell-level metadata, kernelspec, tags, papermill parameter tagging

## Design Philosophy

This skill provides operational tools, not opinions. It is deliberately agnostic about coding conventions, narrative style, and notebook structure — those decisions belong to the user, the team, or other skills that specialise in conventions. This avoids conflicts when composing multiple skills together.

## Installation

Copy the `jupyter-notebooks/` folder into your agent's skills directory.

### Claude Code

```bash
# User-level
cp -r jupyter-notebooks ~/.claude/skills/

# Project-level
cp -r jupyter-notebooks .claude/skills/
```

### Codex (OpenAI)

```bash
# User-level
mkdir -p ~/.agents/skills
cp -r jupyter-notebooks ~/.agents/skills/

# Project-level
mkdir -p .agents/skills
cp -r jupyter-notebooks .agents/skills/
```

### Cursor

```bash
# User-level
mkdir -p ~/.cursor/skills
cp -r jupyter-notebooks ~/.cursor/skills/

# Project-level
mkdir -p .cursor/skills
cp -r jupyter-notebooks .cursor/skills/
```

### Via npx skills CLI

```bash
npx skills add marcinmiklitz/jupyter-notebooks-skill
```

### Via Claude Code plugins

```
/plugin marketplace add marcinmiklitz/jupyter-notebooks-skill
/plugin install jupyter-notebooks
```

## Requirements

- **Python 3.9+**
- Scripts use [PEP 723](https://peps.python.org/pep-0723/) inline metadata, so `uv run` works out of the box with no manual installs.

Core packages (installed automatically by `uv run`):

```
nbformat  nbclient  nbconvert  nbdime
```

Optional:

```
papermill  nbstripout
```

Execution requires a Jupyter kernel (typically `ipykernel`). PDF export requires pandoc/TeX or a webpdf stack.

## Script Overview

| Script | Purpose |
|---|---|
| `nb_create.py` | Create from blank/template/script; inject code into existing notebook |
| `nb_cells.py` | Cell CRUD, reorder, metadata/tags, bulk ops, regex search |
| `nb_execute.py` | Execute with nbclient or papermill, full or selective range |
| `nb_validate.py` | Schema validation + operational lint checks |
| `nb_convert.py` | Export to html/pdf/latex/script/markdown/rst/slides |
| `nb_metadata.py` | Notebook and cell metadata/tag management |
| `nb_outputs.py` | List, strip, extract images, check sizes, clear counts |
| `nb_diff.py` | nbdime-backed text/JSON diff and three-way merge |
| `nb_preflight.py` | Environment readiness check (Python, deps, kernels) |

All scripts emit human-readable logs to **stderr** and machine-readable JSON to **stdout**. Mutating operations require explicit `--in-place` or `--output`.

## Documentation

The skill includes 8 reference guides in `jupyter-notebooks/references/`:

- [Cell Operations](jupyter-notebooks/references/cell-operations.md) — data model, CRUD patterns, bulk ops, safety checks
- [Execution Guide](jupyter-notebooks/references/execution-guide.md) — nbclient vs papermill decision tree, selective range execution, timeout strategy
- [Validation & Linting](jupyter-notebooks/references/validation-and-linting.md) — schema checks, operational lint rules, CI integration
- [Versioning Guide](jupyter-notebooks/references/versioning-guide.md) — nbdime/nbstripout setup, merge strategies, naming conventions
- [Output Handling](jupyter-notebooks/references/output-handling.md) — MIME bundles, strip patterns, image extraction, size estimation
- [Papermill Patterns](jupyter-notebooks/references/papermill-patterns.md) — parameter cells, batch execution, template design, failure handling
- [Metadata Reference](jupyter-notebooks/references/metadata-reference.md) — notebook/cell metadata schema, kernelspec, tags, slideshow
- [Error Handling](jupyter-notebooks/references/error-handling.md) — kernel crashes, timeouts, cell failures, graceful cleanup

For the full agent-facing spec, see [`jupyter-notebooks/SKILL.md`](jupyter-notebooks/SKILL.md).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
