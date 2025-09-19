# ✅ Safe Commit Checklist

Run through this list before staging and committing changes.

## Sensitive Files
- [ ] Did I **exclude** `.env`, `*.tfstate`, `*.pem`, `*.csv`, `*.log` files?
- [ ] Did I check `.gitignore` is up to date for any new file types?
- [ ] No **cloud credentials** or **private keys** are being committed.

## Sensitive Values
- [ ] No AWS Access Key IDs (`AKIA...` or `ASIA...`) in code or docs.
- [ ] No AWS Secrets, IAM inline policies, or KMS keys in plaintext.
- [ ] No ARNs with my real 12-digit AWS Account ID.
- [ ] No GitHub PATs (`ghp_...`), Slack tokens, or JWTs.

## Artifacts / Screenshots
- [ ] Screenshots **redacted** before saving (account IDs, emails, IPs).
- [ ] Diagrams / configs reviewed for secrets before pushing.

## Commit Hygiene
- [ ] Commit message uses conventional commits (`chore:`, `feat:`, `docs:`).
- [ ] Pre-commit hook ran without blocking.
- [ ] Optional: run `gitleaks detect --source .` locally before `git push`.

---

⚠️ If in doubt: **stop, redact, or move the file to .gitignore**.  
