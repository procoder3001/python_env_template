# Python Project Setup Guide

This guide covers setting up a Python development environment using pyenv, poetry, and HashiCorp Vault for secrets management.

## Prerequisites

### Install pyenv on macOS
```sh
# Install pyenv using Homebrew
brew install pyenv

# Add pyenv to your shell (add these to your ~/.zshrc)
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc

# Reload your shell
source ~/.zshrc
```

### Install Poetry
```sh
curl -sSL https://install.python-poetry.org | python3 -
```

### Install HashiCorp Vault
```sh
brew tap hashicorp/tap
brew install hashicorp/tap/hcp

# Login and initialize
hcp auth login
hcp profile init --vault-secrets
```

## Project Setup

1. Create and activate a new Python environment:
```sh
# Install Python version
pyenv install 3.12.2

# Create virtual environment
pyenv virtualenv 3.12.2 project-env-3.12.2

# Activate environment
pyenv activate project-env-3.12.2
```

2. Initialize Poetry project:
```sh
# Initialize new project
poetry init

# Verify environment
poetry env info
poetry run python --version
```

3. Add dependencies:
```sh
# Add main dependencies
poetry add fastapi uvicorn

# Add development dependencies
poetry add -G dev pytest black isort mypy
```

## HashiCorp Vault Integration

```sh
# Open secrets
hcp vault-secrets secrets open {desired secret}

# Run application with injected secrets
hcp vault-secrets run -- python3 my_app.py
```

## Cursor IDE Configuration

Create the following files in your project root:

1. `.cursorignore`:
```text
# Ignore virtual environments
.venv/
venv/
__pycache__/
*.pyc

# Ignore build artifacts
dist/
build/
*.egg-info/

# Ignore environment files
.env
.env.*
```

2. `.cursorrules`:
```json
{
  "python": {
    "formatter": "black",
    "linter": "flake8",
    "typeChecker": "mypy"
  }
}
```

3. `.vscode/settings.json` (for Cursor compatibility):
```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "python.analysis.typeCheckingMode": "basic",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.organizeImports": true
  }
}
```

## Project Structure
