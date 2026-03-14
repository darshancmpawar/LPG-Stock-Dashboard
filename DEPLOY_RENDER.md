# Deploy on Render (Dash + Excel)

This repo uses a **Python web service** (not a static site).

## Why your deploy failed
The error below happens when Render is configured as a **Static Site** and expects a publish directory:

- `Publish directory LPG Stock Dashboard does not exist!`

This app is Dash/Python and must run with Gunicorn.

## Correct setup
1. In Render, create **Web Service** (not Static Site).
2. Connect this repo/branch.
3. Render will auto-detect `render.yaml` from repo root.

If entering manually, use:

- Build command:
  - `pip install -r "LGP web dashbord/requirements.txt"`
- Start command:
  - `gunicorn --chdir "LGP web dashbord" app:server`

## Excel data
The app reads from:
- `LGP web dashbord/data/lpg_stock_data.xlsx`

So keep this file committed in the repo for deployment.
