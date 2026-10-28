# Lab Journal

Short daily notes on what I did, what I learned, and what's next. Newest entries go at the top.

<!--
HOW TO USE: Each day, copy the matching entry below to the top of the "Entries" section,
replace the [brackets] with what actually happened, and delete this comment.
Keep it honest. If something broke or confused you, write that down. It's the most
interesting part to read.
-->

## Entries

### October, 27, 2026: Billing guardrails

**What I did**
- Created a zero-spend budget that emails me if anything costs even $0.01
- Created a $5 monthly cost budget as a second alert

**What I learned**
- Cost alerts are a security control too. A surprise spike in spending is one of the first signs that someone is stealing assets.

**Next:** account alias, password policy, admins group, and an MFA-protected admin user. 

---

### Day 3: A separate admin identity

**What I did**
- Set an account alias and a strict IAM password policy
- Created an `Admins` group with `AdministratorAccess`, and an IAM user in that group
- Turned on MFA for the new user, and created no access keys
- Signed out of root and signed in as the new admin user

**What I learned**
- Giving permissions to a group instead of directly to a user makes access easier to review and remove later.
- [your note]

**Next:** prove the setup is correct from the command line.

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
