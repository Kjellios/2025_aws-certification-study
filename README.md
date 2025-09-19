# AWS + HashiCorp Certification Study Repo

This repository documents my intensive certification study path.  
It contains notes, labs, wrong-answer logs, practice scores, and progress trackers for AWS and HashiCorp certifications.

## 🎯 Goals
- Pass multiple certifications in 12 months using an intensive cadence (25–30h/week).
- Start with **AWS Solutions Architect – Associate (SAA-C03)** and chain vouchers into harder exams.
- Reinforce knowledge through hands-on labs and public documentation.

## 📅 Exam Roadmap
1. SAA-C03 (Solutions Architect – Associate)
2. SCS-C02 (Security Specialty)
3. Terraform Associate (003)
4. SAP-C02 (Solutions Architect – Professional)
5. ANS-C01 (Advanced Networking Specialty)
6. DAS-C01 (Data Analytics Specialty)

## 📂 Repo Structure
```plaintext
aws-certification-study/
├── 00_admin/                # Global planning and templates
│   ├── study-roadmap.md     # 12-month plan + voucher tracking
│   ├── daily-schedule.md    # Daily cadence template
│   ├── wrong-answer-log-template.md
│   ├── lab-journal-template.md
│   └── progress-tracker.xlsx
├── 01_saa-c03/              # Phase 1 – Solutions Architect Associate
│   ├── notes/               # Trainer notes + recall bullets
│   ├── labs/                # Step-by-step hands-on builds
│   ├── artifacts/           # Diagrams, screenshots, configs
│   └── wrong-answers/       # Weekly wrong-answer logs
├── 02_scs-c02/              # Phase 2 – Security Specialty
│   └── ...
├── 03_terraform-associate/  # Phase 3 – Terraform
│   └── ...
├── 04_sap-c02/              # Phase 4 – Solutions Architect Professional
│   └── ...
├── 05_ans-c01/              # Phase 5 – Advanced Networking Specialty
│   └── ...
├── 06_das-c01/              # Phase 6 – Data Analytics Specialty
│   └── ...
├── template/                # File format templates (notes, labs, wrong-answers, artifacts)
└── .gitignore
```

## 🛠 Using Templates

- **When you watch videos →** copy `template/notes/TEMPLATE-notes.md` into the current exam’s `notes/` folder and fill it in.  
- **When you do a lab →** copy `template/labs/TEMPLATE-lab.md` into the current exam’s `labs/` folder.  
- **When you miss a question →** copy `template/wrong-answers/TEMPLATE-wrong-answer-log.md` into the current exam’s `wrong-answers/` folder for that week.  
- **Artifacts** (diagrams, screenshots) go into `artifacts/`.

This keeps every file consistent across phases.

## ⚡ Workflow
- **Daily:** Watch videos → do labs → log wrong answers → push commit.
- **Weekly:** Update progress tracker, review weak areas.
- **Commit messages:**  
  ```
  [SAA Week1-Day1] IAM role lab + 25Q quiz logged
  ```
- **Security:** `.gitignore` excludes credentials, `.tfstate`, `.env`. Never commit secrets.

---

## ✅ Readiness Gates
- Two full-length practice exams ≥70–75% in 7 days.
- All wrong-answer log items closed.
- Able to whiteboard key architectures without notes.
