# Deploying ESG-Scoring-App (Voila)

This project currently uses a Jupyter notebook UI (`app.ipynb`) with `ipywidgets`.
The fastest deployment path is serving the notebook with **Voila**.

## 1) Local smoke test

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
voila app.ipynb --port=8866 --no-browser --Voila.ip=0.0.0.0
```

Open `http://localhost:8866`.

## 2) Deploy on Render (quickest hosted option)

1. Push this repo to GitHub.
2. In Render, create a new **Web Service** from the repo.
3. Configure:
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `voila app.ipynb --port=$PORT --no-browser --Voila.ip=0.0.0.0`
4. Deploy.

Render will detect `Procfile` automatically in many setups; using explicit build/start commands is still recommended.

## 3) Deploy on Railway

1. Create a project from your GitHub repo.
2. Railway will install dependencies from `requirements.txt`.
3. Set start command to:

```bash
voila app.ipynb --port=$PORT --no-browser --Voila.ip=0.0.0.0
```

## Notes for production

- `investor_rules.json` is file-based state. Some hosts use ephemeral filesystems, so data may reset on redeploy/restart.
- For persistence, move rules to a database or external storage.
- The current login is UI-only (no secure auth backend). Add proper authentication before public production use.


## 4) If your host is using Docker builds

If your deploy logs show an error like:

```text
failed to read dockerfile: open Dockerfile: no such file or directory
```

your platform is attempting a Docker build. This repository now includes a `Dockerfile`.

- Ensure your service is configured to use the **repo root** as the build context.
- Keep `Dockerfile` at the root of the repo.
- Re-deploy after pushing the latest commit.

The Docker image starts Voila with:

```bash
voila app.ipynb --port=${PORT} --no-browser --Voila.ip=0.0.0.0
```

