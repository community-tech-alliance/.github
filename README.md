## Notice

This repository is maintained by Community Tech Alliance for internal use.  
It is public for transparency, but **not officially supported for external use**.  
See [LICENSE](./LICENSE) for terms.

---

# 📘 Using GitHub Composite Actions & Reusable Workflows

GitHub Actions offers two ways to modularize your CI/CD pipelines: **composite actions** and **reusable workflows**. Both help simplify and reuse logic across multiple workflows or repositories.

---

## 🔁 Composite Actions

### ✅ What They Are
Composite actions let you bundle multiple steps (like shell scripts or other actions) into a single custom action. Ideal for **step-level reuse** within the same or different repositories.

### 📂 Directory Structure
```
.github/
└── actions/
    └── my-composite-action/
        ├── action.yml
        └── script.sh
```

### 📄 `action.yml` Example
```yaml
name: "My Composite Action"
description: "Runs a script and checks output"
inputs:
  name:
    required: true
    description: "Name to greet"
runs:
  using: "composite"
  steps:
    - run: echo "Hello, ${{ inputs.name }}!"
      shell: bash
```

### 🚀 How to Use
In your workflow:
```yaml
uses: ./.github/actions/my-composite-action
with:
  name: "World"
```

---

## 🔁 Reusable Workflows

### ✅ What They Are
Reusable workflows allow you to call **entire workflows** from other workflows. Great for **workflow-level reuse** across repos or jobs.

### 📂 Directory Structure
```
.github/
└── workflows/
    └── reusable-workflow.yml
```

### 📄 `reusable-workflow.yml` Example
```yaml
name: "Reusable Build Workflow"
on:
  workflow_call:
    inputs:
      buildTarget:
        required: true
        type: string
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building ${{ inputs.buildTarget }}"
```

### 🚀 How to Call It
From another workflow:
```yaml
jobs:
  call-reusable:
    uses: org/repo/.github/workflows/reusable-workflow.yml@main
    with:
      buildTarget: "frontend"
```

---

## 🔍 Key Differences

| Feature             | Composite Actions                      | Reusable Workflows                         |
|---------------------|----------------------------------------|--------------------------------------------|
| Scope               | Steps                                  | Entire workflows                           |
| Location            | `.github/actions/`                     | `.github/workflows/`                       |
| Triggerable by      | `uses:` in a step                      | `uses:` in a job                           |
| Inputs/Outputs      | Yes (step-level)                       | Yes (workflow-level)                       |
| Reuse Across Repos  | Yes (via GitHub repo ref)              | Yes (via repo and branch ref)              |
| Shell Scripts       | Native support                         | Supported inside steps                     |

---

## ✅ Best Practices

- Use **composite actions** for bundling shell commands or small routines.
- Use **reusable workflows** for shared CI/CD pipelines or multi-job templates.
- Version your shared logic via branches or tags for stability.
- Keep documentation and input/output definitions clear.

---

