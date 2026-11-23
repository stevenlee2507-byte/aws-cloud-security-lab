# IR-001: Public S3 Bucket Exposure (Lab Simulation)

**Severity:** Medium **Status:** Closed
**Resource:** seclab-public-test-6767   **Analyst:** Steven Lee

## 1. Summary
A test S3 bucket was made publicly readable through a bucket policy
(Principal "*", s3:GetObject). The change was detected by a CloudWatch alarm
and IAM Access Analyzer, investigated in CloudTrail, and fixed. Only one
harmless test file was exposed.

## 2. Timeline (UTC)
| Time | Event |
|------|-------|
| 2026-11-18 20:30 | Block Public Access turned off (PutBucketPublicAccessBlock) |
| 2026-11-18 20:36 | Public bucket policy added (PutBucketPolicy) |
| 2026-11-18 20:27 | ALARM email from seclab-s3-policy-changes |
| 2026-11-18 21:02 | IAM Access Analyzer finding created (Active) |
| 2026-11-18 21:02 | Investigation in CloudTrail |
| 2026-11-23 13:35 | Policy deleted, Block Public Access restored |
| 2026-11-23 13:51 | Access Analyzer finding Resolved |

## 3. Detection
CloudWatch alarm seclab-s3-policy-changes (metric filter on S3 policy events)
and IAM Access Analyzer (external access finding).

## 4. Investigation
CloudTrail PutBucketPolicy event: userIdentity = steven-admin,
sourceIPAddress = [161.115.114.179], eventTime = [21:02 UTC]. The identity and IP matched
the lab admin session, so this was an authorized test, not an outside attacker.

## 5. Root cause
Block Public Access was disabled on the bucket, which allowed a public
bucket policy to take effect.

## 6. Remediation
Deleted the bucket policy, re-enabled Block Public Access (all 4 settings),
rescanned in Access Analyzer, and confirmed the object URL returns AccessDenied.

## 7. Prevention
Keep account-level Block Public Access on. Keep the S3 policy alarm and
Access Analyzer running. Run scripts/seclab-audit.sh regularly (Session 21).

## 8. Framework mapping
CIS AWS Foundations v3.0.0: 4.8 (S3 bucket policy changes monitored),
2.1.4 (Block Public Access - verify). MITRE ATT&CK: T1530 Data from Cloud Storage.

## 9. Evidence
screenshots 30-35 

![bucket public](screenshots/30-test-bucket-public.png)
![incognito proof](screenshots/31-public-bucket-proof.png)
![analyzer finding](screenshots/32-IAM-access-analyzer-findings.png)
![details of policy change](screenshots/33-putback-policy-findings.png)
![fixed bucket](screenshots/34-bucket-public-access-fix.png)
![resolved finding](screenshots/35-finding-resolved.png)
