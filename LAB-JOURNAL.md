# Real world analysis

First started as journal entry of the day but are now real world comparisons of the situations.

## Entries

### October, 27, 2026: Billing guardrails

**What I did**
- Created a zero-spend budget that emails me if anything costs even $0.01
- Created a $5 monthly cost budget as a second alert

**What I learned**
- Cost alerts are a security control too. A surprise spike in spending is one of the first signs that someone is stealing assets.

---

### October 28, 2026: A separate admin identity

**How this fits in the real world**
- Root: The root user can do anything, even close the account or change billing, and no rules can limit it. In this situation it felt like keeping the master key locked in a safe and using a regular key every day. In real companies root is locked with MFA and barely used, so if a login gets stolen it isn't game over.
- MFA: Stolen passwords are one of the most common ways hackers get in. In this case MFA is my backup lock, because even if someone has my password they still can't log in without my fingerprint.
- No access keys: Access keys are permanent and can get leaked on GitHub or saved on laptops. In the 2016 Uber breach, hackers found AWS keys in a private GitHub repo. By not making any keys, there's nothing to leak.
- Groups: Giving permissions to a group instead of each person makes it easier to see who has access and remove it when someone leaves. At a big company this stops old accounts from keeping access they shouldn't have.

---

### Day 4: Verifying the setup with the AWS CLI

**What I did**
- Used AWS CloudShell, a terminal in the browser, to run verification commands with no keys on my laptop
- Confirmed root MFA is on and that no root access keys exist
- Confirmed the password policy, admin MFA, and budgets from the CLI
- Added a security alternate contact to the account

**What I learned**
- Checking settings with commands is more reliable than clicking around, and it's the first step toward automating security checks.
- [your note]

**Next:** finish the Phase 1 write-up.

---

### Day 5: Phase 1 complete

**What I did**
- Finished the Phase 1 write-up with a checklist of every control and its evidence
- Updated the main README progress

**What I learned**
- [biggest takeaway from Phase 1]
- [what you'd do differently]

**Next:** Phase 2, turning on CloudTrail and GuardDuty.
