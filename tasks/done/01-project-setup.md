# 01 - Project Setup

## Background
Nothing exists yet. We need a Python package, a CLI entry point and a place for tests, so every later task has somewhere to land.

## What to do
1. Create a Python 3.10+ package `echobook/` (lowercase is the Python convention).
2. Add `pyproject.toml` with the CLI entry point `echobook = echobook.cli:main` (and `echopage` alias).
3. Dependencies: `lxml`, `beautifulsoup4`, `nltk` (or `spacy`), `click` or `argparse` (stdlib is fine), `pytest`. Leave WhisperX for task 07.
4. Create empty modules: `cli.py`, `decryptor.py`, `parser.py`, `aligner.py`, `packager.py`.
5. Add `.gitignore` (venv, `__pycache__`, work dirs, `dist/`, `*.epub`, audio files).
6. Add a README that says how to install FFmpeg (`brew install ffmpeg`) and the package.

## Acceptance Criteria
- [x] `uv pip install -e .` succeeds in a fresh virtualenv.
- [x] `echobook --help` runs and prints usage.
- [x] `pytest` runs (even with 0 or 1 placeholder test) and exits 0.
- [x] `ffmpeg -version` is documented as a prerequisite.
