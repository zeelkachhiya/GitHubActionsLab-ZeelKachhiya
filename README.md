# GitHub Actions Lab — GitHubActionsLab-ZeelKachhiya

This repository contains a simple **Java Ant project** and two **GitHub Actions** workflows created for the GitHub Actions Lab.

## Repository Structure

- `.github/workflows/dependent-jobs.yml` — Workflow 1 (Job dependencies)
- `.github/workflows/multi-platform.yml` — Workflow 2 (Multi-platform testing)
- `src/`, `build.xml`, `nbproject/` — Java Ant project files (created in NetBeans)

---

## Workflow 1: Dependent Jobs (`dependent-jobs.yml`)

### Purpose
Demonstrates **job dependencies** using the `needs` keyword so jobs run in a strict order:

**build → test → deploy**

### Trigger
Runs on **push to the `master` branch**.

### What it does
- **build** (runs on `ubuntu-latest`)
  - prints “Building application…”
  - simulates work using `sleep`
- **test** (runs on `ubuntu-latest`, depends on `build`)
  - prints “Running tests…”
  - simulates work using `sleep`
- **deploy** (runs on `ubuntu-latest`, depends on `test`)
  - prints “Deploying application…”
  - simulates work using `sleep`

---

## Workflow 2: Multi-Platform Testing (`multi-platform.yml`)

### Purpose
Demonstrates **parallel jobs** across multiple operating systems (no dependencies between jobs).

### Trigger
Runs on **pull_request to the `master` branch**.

### What it does (in each OS job)
Each job:
1. Checks out the code using `actions/checkout@v4`
2. Prints OS information
3. Runs an OS-specific command (Linux/macOS: `uname -a`, Windows: `systeminfo`)
4. Creates a simple file and prints its contents

Jobs included:
- `ubuntu-job` → `ubuntu-latest`
- `windows-job` → `windows-latest`
- `macos-job` → `macos-latest`

---

## Key GitHub Actions Concepts Demonstrated

### `runs-on`
Defines which runner (operating system) a job uses, for example:
- `ubuntu-latest`
- `windows-latest`
- `macos-latest`

### `needs`
Creates job dependencies and forces execution order.
Example idea:
- `test` **needs** `build`
- `deploy` **needs** `test`

If a job does **not** use `needs`, it can run in **parallel** with other jobs.

### `env`
`env` allows you to define environment variables at different levels (workflow / job / step).
Example usage pattern:

```yml
env:
  DEMO_MESSAGE: "Hello from env"

jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Print env variable
        run: echo "$DEMO_MESSAGE"
