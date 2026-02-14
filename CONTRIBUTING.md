# Contributing

Contributions are welcome. This guide covers how to report issues, suggest improvements, and submit changes.

## Reporting Issues

Open a [GitHub issue](https://github.com/marcinmiklitz/jupyter-notebooks-skill/issues) with:

- What you tried (command, agent, platform)
- What happened (error message, unexpected output)
- What you expected
- Python version and OS

## Suggesting Features

Open an issue describing the use case. Include example commands or workflows if possible.

## Submitting Changes

1. Fork the repo and create a branch from `main`.
2. Make your changes.
3. Ensure all scripts still run with `--help` (see testing below).
4. Submit a pull request with a clear description of the change.

## Project Structure

```
jupyter-notebooks/          # The skill (this is what gets installed)
├── SKILL.md                # Agent-facing spec and routing table
├── scripts/                # Python CLI tools (PEP 723 inline deps)
├── references/             # Detailed guides for agents
└── assets/
    ├── templates/          # Notebook templates (.ipynb)
    └── .gitattributes.example
```

## Script Conventions

All scripts follow the same contract:

- **PEP 723 inline metadata** for dependencies (no requirements.txt)
- **stderr** for human-readable status messages
- **stdout** for machine-readable JSON output
- **Exit codes**: `0` success, `1` error (`nb_validate.py`: `0` clean, `1` issues, `2` tool failure)
- **Mutating operations** require `--in-place` or `--output` (no silent overwrites)
- Every script must work with `uv run scripts/<name>.py --help`

## Testing

Run `--help` on every script to verify they parse correctly:

```bash
for script in jupyter-notebooks/scripts/nb_*.py; do
    uv run "$script" --help > /dev/null || echo "FAIL: $script"
done
```

If you modify a script, test it against a real notebook:

```bash
# Create a test notebook
uv run jupyter-notebooks/scripts/nb_create.py --template blank --output /tmp/test.ipynb

# Run your modified script against it
uv run jupyter-notebooks/scripts/<your_script>.py --input /tmp/test.ipynb <args>
```

## Style

- Python code: follow existing patterns in the scripts (argparse, `status()`/`emit()`/`fail()` helpers)
- Markdown: keep reference docs focused and example-heavy
- SKILL.md: update the script map and routing table if adding new capabilities

## License

By contributing, you agree that your contributions will be licensed under the [MIT License](LICENSE).
