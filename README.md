# FastAPI Beyond CRUD

A REST API for a book review web service built with FastAPI, PostgreSQL, Redis, and Celery.

## Running the Application

```bash
docker compose up
```

This starts all services (web, database, Redis, Celery worker) with no extra setup required. The API is available at `http://localhost:8000/api/v1/docs`.

## Running Tests

```bash
pytest src/tests/ -v
```

## CI/CD

### Conventional Commits Check

A GitHub Actions workflow runs on every pull request to `main`. It validates that all commits follow the [Conventional Commits](https://www.conventionalcommits.org/) format (e.g. `feat: add endpoint`, `fix(auth): token expiry`). If any commit fails validation, the PR is automatically closed with a comment explaining the required format.

### Nightly Build

A scheduled workflow runs at midnight (UTC) every day:
1. Runs the test suite
2. If tests pass, builds a Docker image and pushes it to GitHub Container Registry (`ghcr.io`)
3. If tests fail, the image is not pushed and a failure notification email is sent via [Ethereal Email](https://ethereal.email/)

The nightly build can also be triggered manually from the Actions tab.

## Changes Made for Assignment 07

1. Added `.github/workflows/conventional-commits.yml` — PR conventional commit validation with auto-close on failure
2. Added `.github/workflows/nightly-build.yml` — nightly Docker build with test gating, GHCR push, and email notification on failure
3. Added `.env` — environment configuration with Ethereal Email credentials so `docker compose up` works out of the box
4. Added `pytest` to `requirements.txt` — was missing as a dependency despite tests existing
5. Removed `.env` from `.gitignore` — allows the `.env` file (containing only test/demo credentials) to be committed
6. Configured GitHub repository secrets (`ETHEREAL_USERNAME`, `ETHEREAL_PASSWORD`, `ETHEREAL_EMAIL`) for CI email notifications
