# 🔒 Repo Security Hardening Checklist

This document tracks the guardrails in place to prevent sensitive values from leaking into Git history or GitHub.

---

## ✅ Completed

- **.gitignore hardened**  
  - Excludes `.env`, `*.tfstate`, `.pem`, `.log`, and other sensitive files.

- **Pre-commit hook**  
  - Blocks AWS Access Key IDs, AWS Secrets, ARNs with account IDs, and 12-digit AWS Account IDs.  
  - Regex fixed and tested with fake keys.  
  - Hook lives at `.git/hooks/pre-commit`.

- **Safe Commit Checklist**  
  - File: `00_admin/safe-commit-checklist.md`.  
  - Human-layer checklist before every commit.  
  - Covers sensitive files, values, screenshots, commit hygiene.

---

## ⬜ Still To Do

- **4. GitHub Actions secret scanning**  
  - Add `.github/workflows/secret-scan.yml` with `gitleaks`.  
  - Detects GitHub PATs (`ghp_...`), Slack tokens, JWTs, and 100+ other formats.  

- **5. Branch protection rules (GitHub UI)**  
  - Protect `main` branch.  
  - Require passing checks (secret scan, CI) before merge.  
  - Optional: require PR reviews.

- **6. GitHub Secret Scanning & Push Protection**  
  - Confirm enabled under: Settings → Code security and analysis.  
  - Enable *Push Protection* if available.

- **7. Optional pre-push hook**  
  - Local hook to run `gitleaks detect --source .` before every push.

- **8. Habit layer**  
  - Redact screenshots, replace IDs with placeholders.  
  - Store private notes in `00_admin/` (don’t commit if sensitive).

---

⚠️ Rule: No exam study notes, artifacts, or labs should ever include real AWS secrets, account IDs, or unredacted screenshots.
