# AWS Cloud Security Lab

A hands-on lab where I'm building a secure AWS environment from scratch, turning on AWS's detection tools, simulating common cloud misconfigurations and attacks, and detecting and fixing each one.

I'm building this in public. Daily progress notes are in [LAB-JOURNAL.md](LAB-JOURNAL.md).

## Progress

- [ ] **Phase 1: Account hardening**: root MFA, billing guardrails, a separate admin identity, account verification *(in progress)*
- [ ] **Phase 2: Detection setup**: CloudTrail, GuardDuty, AWS Config, Security Hub, IAM Access Analyzer
- [ ] **Phase 3: Attack and misconfiguration scenarios**: break, detect, fix, verify
- [ ] **Phase 4: Results, lessons learned, and teardown**

## Skills this lab covers

- Identity and access management (MFA, least privilege, password policy)
- Cloud cost and risk guardrails (budgets and alerts)
- Logging, monitoring, and threat detection *(Phase 2)*
- Incident triage and MITRE ATT&CK mapping *(Phase 3)*

## Repo layout

| Folder | What's inside |
| --- | --- |
| `01-account-hardening/` | Phase 1 write-up, redacted screenshots, and CLI verification output |

More folders get added as each phase starts.

## Lab rules I'm following

- Everything runs in one region: `us-east-1`
- No real or personal data goes into anything I expose on purpose
- Every screenshot and output is redacted before upload (account ID, emails, IPs, keys)
- No access keys are ever created for the admin user or root
