# M&M SmartShop Hybrid Recommender

This project upgrades the original M&M storefront into a real recommender-system demo.
Instead of serving mostly precomputed JSON outputs, the app now:

- cleans the raw Excel datasets once into `data/cleaned_dataset.xlsx`,
- loads the cached cleaned workbook on app startup,
- builds a hybrid baseline recommender,
- optimizes the final top-5 list with a genetic algorithm,
- exposes baseline vs optimized recommendations in the UI,
- and evaluates the system offline with holdout metrics.

## Main Features

- Hybrid baseline using:
  - category preference,
  - price alignment,
  - similar-user support,
  - popularity,
  - average rating quality.
- Genetic Algorithm over the candidate pool to improve:
  - diversity,
  - novelty,
  - relevance balance,
  - final list quality.
- Flask storefront demo with:
  - login by dataset `user_id`,
  - recommendation comparison panels,
  - GA tuning controls and fitness-history trace,
  - search,
  - demo cart,
  - demo rating capture.
- Offline evaluation output in `docs/evaluation_summary.json`.

## Project Structure

- `app.py`: Flask routes and API endpoints.
- `src/data_cleaning.py`: preprocessing pipeline that exports the cleaned workbook.
- `src/recommender.py`: hybrid recommender + GA engine.
- `src/evaluation.py`: holdout evaluation pipeline.
- `templates/`: HTML pages.
- `static/`: CSS and JavaScript.
- `docs/`: report, video script, checklist, and evaluation summary.
- `data/`: dataset files.
- `wsgi.py`: production entry point.
- `render.yaml`: Render deployment config.

## Run Locally

```bash
pip install -r requirements.txt
python -m src.data_cleaning
python -m src.evaluation
python app.py
```

Then open `http://127.0.0.1:5000`.

## Current Evaluation Snapshot

From `docs/evaluation_summary.json`:

- `precision@5`: baseline `0.0044`, optimized `0.0056`
- `recall@5`: baseline `0.0111`, optimized `0.0130`
- `diversity@5`: baseline `0.4167`, optimized `0.7511`
- `coverage`: baseline `0.4320`, optimized `0.4880`
- `hit_rate`: baseline `0.0222`, optimized `0.0278`

## Deployment

This repository includes:

- `render.yaml`
- `wsgi.py`
- `gunicorn` in `requirements.txt`

Typical Render start command:

```bash
gunicorn wsgi:app
```
