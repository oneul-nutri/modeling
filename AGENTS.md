# Repository Guidelines

## Project Structure & Module Organization
Top-level work happens inside `food-calorie-vision-crawling/`, which houses automation scripts (`scripts/`), YOLO configs (`configs/`), lightweight checkpoints under `models/`, run outputs in `data/`, and label exports inside `labels/`. Database-to-markdown experiments live in `db-preprocessing/` (`preprocessing.ipynb`, CSV/MD folders), while `data-preprocess-main.ipynb` is a scratchpad for exploratory cleaning. Group raw crawls and assets by `run_id` (e.g., `data/raw/20250107_poc_a`) so filtering, labeling, and training stages stay aligned.

## Build, Test, and Development Commands
Use a dedicated Python environment per repo; install crawler/training deps and Playwright binaries before launching pipelines. The commands below cover the usual flow; adjust flags as needed but log every invocation.
```bash
cd food-calorie-vision-crawling
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt && playwright install chromium
UV_CACHE_DIR=.uv-cache uv run python scripts/crawl_images_playwright.py --run-id 20250107_poc_a --classes 기장 보리 조 --min_per_class 50 --max_per_class 80 --show-browser
python scripts/dedup_filter.py --input data/raw/20250107_poc_a --output data/filtered/20250107_poc_a
python scripts/prepare_food_dataset.py --run-id 20250107_poc_a --source labels/yolo_validated/20250107_poc_a --label-map food_class.csv --val-ratio 0.2
UV_CACHE_DIR=.uv-cache uv run python scripts/train_yolo.py --run-id 20250107_poc_a --config configs/food_poc.yaml --model models/yolo11l.pt --device 0
```

## Coding Style & Naming Conventions
Python sources follow PEP 8 (4-space indents, snake_case functions, UpperCamelCase classes) with type hints for new modules. Keep CLI entry points idempotent, fail fast on invalid paths, and surface JSON/CSV summaries in `data/meta/`. Reuse the `YYYYMMDD_stage_letter` scheme for run IDs across folders, commits, and stats. Markdown files should use UTF-8 and fenced code blocks for reproducible commands.

## Testing Guidelines
No formal unit suite exists yet, so lean on script-level guards: use `--dry-run` for crawlers, confirm `data/filtered/<run_id>/stats.yaml`, and run `python scripts/visualize_predictions.py --run-id <run_id> --top-n 2 --bottom-n 3` before packaging datasets. When editing notebooks, copy them and describe validation steps in Markdown cells.

## Commit & Pull Request Guidelines
Adopt the Conventional Commit prefixes visible in the log (`feat:`, `fix:`, `chore:`, etc.) and keep subjects under 72 characters. Each PR should describe the affected run IDs, key scripts executed, and attach metrics or screenshots (e.g., sample detections). Link related issues, list manual validation steps, and call out any follow-up tasks required for downstream teams.

## Security & Data Handling
Never commit downloaded images, proprietary datasets, or secrets; only metadata and configs belong in git. Record licensing and provenance updates in `food-calorie-vision-crawling/data/source_list.md`, scrub credentials from notebooks, and rotate any API keys in your local environment rather than in the repository.
