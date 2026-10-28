# Phase 1: Account Hardening

**Goal:** Before building anything, make the AWS account safe to use. By the end of this phase, the root user is locked away, a surprise bill isn't possible, and day-to-day work happens through a separate admin user protected by MFA.

**Status:** In progress <!-- Change to "Complete" on Day 5 -->

## Controls summary

<!-- Fill in the Evidence column as you finish each control. -->

| # | Control | Why it matters | Evidence |
| --- | --- | --- | --- |
| 1 | MFA on the root user | Root has unlimited power and can't be restricted | `screenshots/01-root-mfa.png` |
| 2 | Zero-spend budget | Alerts on any unexpected charge | `screenshots/02-zero-spend-budget.png` |
| 3 | $5 monthly cost budget | Second cost safety net | `screenshots/03-monthly-budget.png` |
| 4 | Strict IAM password policy | Stops weak passwords for any IAM user | `screenshots/04-password-policy.png` |
| 5 | Admin access through a group | Permissions are easy to review and remove | `screenshots/05-admins-group.png` |
| 6 | MFA on the admin user | Protects the identity used every day | `screenshots/06-admin-mfa.png` |
| 7 | No access keys for root or admin | No long-lived secrets that can leak | `evidence/account-summary.txt` |
| 8 | Security alternate contact | AWS can reach me about security issues | `screenshots/07-security-contact.png` |

## 1. Locking down the root user

10/26/26 8:00PM

- My passkey MFA: I registered a passkey that unlocks with my fingerprint for convenience but also for the security of my physical presence being a must to unlock my account
- My Access Key: I decided not to use access keys thanks to the root user

- My notes: In this situation I realized the danger of having a root user that controls all my systems which is of course a minus from the convenience of it. So the first idea that came up to help secure my root user is a fingerprint passkey which I believe is a reliable security method thanks to the necessity of the owners physical presence.

![Root MFA](screenshots/01-root-mfa.png)

## 2. Billing guardrails

10/27/26 8:04PM

2 budgets were made today:
- My zero spend budget: Created with the purpose of being a system that alerts my email in case of a purchase.
- My $5 monthly budget: Created with the purpose of alerting my email if the time comes where I run out of free credits.

- My notes: In this situation it felt like a security situation where I just made my own security control against any unnecessary/unwanted purchases. In this case the cost alerts being my security controls. In this case the unnecessary purchases can be seen similar to a scenario where hackers (AWS) attack my bank's institution to steal my funds. 

![Zero-spend budget](screenshots/02-zero-spend-budget.png)
![Monthly budget](screenshots/03-monthly-budget.png)

## 3. A separate admin identity

<!-- Day 3 -->

[Why you stopped using root, why permissions go through a group, what's in your password policy]

![Password policy](screenshots/04-password-policy.png)
![Admins group](screenshots/05-admins-group.png)
![Admin MFA](screenshots/06-admin-mfa.png)

**Why an IAM user and not IAM Identity Center?** Identity Center is what most companies use, because it gives short-lived credentials. It requires AWS Organizations, though, and this lab runs on a single Free plan account. A single IAM admin user with MFA and no access keys is the safe option for this setup. [Replace this paragraph if your account did support Identity Center.]

## 4. Verifying from the command line

<!-- Day 4: Paste your redacted CLI output into the evidence/ folder and summarize the results here. -->

I used AWS CloudShell so I didn't need to store any access keys on my laptop.

| Check | Command | Result |
| --- | --- | --- |
| Signed in as admin, not root | `aws sts get-caller-identity` | [ARN ends in `user/<your-admin-name>`] |
| Root MFA on, no root keys | `aws iam get-account-summary` | [AccountMFAEnabled = 1, AccountAccessKeysPresent = 0] |
| Password policy applied | `aws iam get-account-password-policy` | [Min length 14, all character types required] |
| Admin MFA registered | `aws iam list-mfa-devices --user-name <name>` | [1 device] |
| Budgets exist | `aws budgets describe-budgets ...` | [2 budgets] |

Full output: [`evidence/`](evidence/)

## What I learned

<!-- Day 5 -->

- [Biggest takeaway]
- [Something that was harder or different than expected]
- [What you'd do differently in a real company]

## Next

Phase 2: turning on CloudTrail, GuardDuty, AWS Config, Security Hub, and IAM Access Analyzer.
