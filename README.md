# Zephr - Agentic Kubernetes Incident Response

## Secret scanning

Each teammate must install local hooks after cloning; Git does not copy installed
hooks. Install Git, Python 3.10+, and [Gitleaks 8.30.1](https://github.com/gitleaks/gitleaks/releases/tag/v8.30.1)
first. For Windows, extract the official Windows x64 ZIP to a user tools directory,
verify its SHA-256 against the release checksums, and add that directory to your
user PATH. Reopen PowerShell, then run from the repository:

```powershell
python -m venv .venv
gitleaks version
.\.venv\Scripts\python.exe -m pip install pre-commit
.\.venv\Scripts\python.exe -m pre_commit install --install-hooks
.\.venv\Scripts\python.exe -m pre_commit run --all-files --hook-stage manual
```

The official `gitleaks-system` hook uses Gitleaks from PATH; keep it at 8.30.1
to match CI. First installation requires internet access.
The manual scan checks the working directory, including untracked and ignored
files (so local credentials can also be reported). Normal commits scan staged changes.
Use `.env.example` with placeholders only; keep real credentials in ignored local
files. If a real secret is detected, remove it and revoke or rotate it.

CI runs the `gitleaks` check on pull requests, pushes to `main`, and manual runs.
The personally owned repository needs no Gitleaks license key. Findings are
redacted, with PR comments and report uploads disabled.
