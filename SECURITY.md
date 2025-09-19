# SECURITY.md – GitHub Security Setup

This document explains the GitHub-specific security configuration for this repository. It is written to be comprehensive enough to serve as both documentation and a step-by-step recreation guide.

---

## 1. GitHub Advanced Security

We enabled GitHub’s built-in security features under **Settings → Code security & analysis**:

- **Secret scanning**: Detects credentials and sensitive patterns in commits.
- **Code scanning**: Supports running security tools like CodeQL.
- **Dependabot alerts**: Notifies when dependencies have known vulnerabilities.
- **Dependabot security updates**: Can automatically open pull requests to fix vulnerable dependencies.

> ⚠️ Note: Even though this project does not use many dependencies, enabling these ensures GitHub flags issues as soon as they are detected.

---

## 2. Secret Scan Workflow (CI)

A GitHub Actions workflow was created to run **Gitleaks** on every push and pull request to `main`.  
This provides an additional server-side layer of enforcement in case local hooks are bypassed.

**File:** `.github/workflows/secret-scan.yml`

```yaml
name: Secret Scan

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Run Gitleaks
        uses: zricethezav/gitleaks-action@v2
        with:
          args: detect --source . --no-git --redact
```

### Steps to Add or Recreate

1. Ensure `.github/workflows` exists.
2. Add the above file at `.github/workflows/secret-scan.yml`.
3. Stage and commit it (force add in case of ignore rules):

   ```bash
   git add -f .github/workflows/secret-scan.yml
   git commit -m "chore: add secret scan workflow"
   git push origin main
   ```
4. Confirm the workflow is registered:

   ```bash
   gh workflow list
   ```

   Expected output should include **Secret Scan** in `active` state.

---

## 3. Workflow Verification

We verified the workflow works correctly:

1. **Trigger Test**

   * Added a harmless change to `README.md`.
   * Pushed commit → confirmed **Secret Scan** ran under GitHub Actions.

2. **Fake Secret Test**

   * Tried committing/pushing a fake AWS key.
   * Workflow correctly flagged it when the push was attempted.
   * Local hooks had already blocked it, but the GitHub job ensures a second layer of defense.

✅ Both verification tests confirmed CI scanning works.

---

## 4. Branch Protection Rules

Configured branch protection for `main` under **Settings → Branches**:

* Require pull request reviews before merging.
* Require status checks to pass (Secret Scan must succeed).
* Disallow direct force pushes or deletion of `main`.
* (Optional, recommended) Require signed commits.

> These rules ensure that unsafe code cannot land in `main` without review and a passing security scan.

---

## 5. Dependabot Configuration

In **Settings → Code security & analysis**, we enabled:

* **Dependabot alerts** (notifies when vulnerabilities are detected).
* **Dependabot security updates** (automatically opens PRs with fixes).

While this repo has minimal dependencies, enabling these ensures future safety.

---

## 6. Recreation Checklist

If this repo is ever cloned fresh or forked, recreate GitHub security as follows:

1. Enable **GitHub Advanced Security features**:

   * Secret scanning
   * Code scanning
   * Dependabot alerts + security updates
2. Add the **Secret Scan workflow** (`.github/workflows/secret-scan.yml`) and push it.
3. Apply **branch protection rules** to `main`.
4. Confirm the workflow is active:

   ```bash
   gh workflow list
   ```

   Look for `Secret Scan` with status `active`.
5. Run a test push with a safe change to trigger the workflow.
6. (Optional) Run a test push with a fake secret to confirm it gets flagged.

---

## 7. Summary of Protections

* **Advanced Security**: Secret scanning + code scanning + Dependabot.
* **GitHub Actions**: Gitleaks workflow active on every push/PR.
* **Branch Protection**: Reviews and passing checks required on `main`.
* **Verification**: Confirmed with both safe and secret test commits.

With these controls, the GitHub side enforces security independently of the developer’s machine. Even if local hooks are removed or ignored, GitHub will scan pushes, block unsafe merges, and surface alerts.