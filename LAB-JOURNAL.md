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

### November 2, 2026: Consolelogin events


**How this fits in the real world**
- CloudTrail: CloudTrail is like a security camera for my AWS account. It keeps track of who did what and when. Event history only saves 90 days, so I made a trail that saves everything to S3. In real hacks, attackers can be inside for months before anyone notices, so if the logs are gone there's nothing to look back at.
- Log file validation: Hackers usually try to delete or turn off logs to hide what they did. Validation lets me prove if any log was changed or deleted.
- Wrong event: The first event I opened had no user and no error. It turned out to be CloudTrail checking its own bucket, not a person. Real analysts deal with this all the time, since most logs are just AWS services talking to each other. The skill is filtering down to what a person actually did.
- Regions: My failed logins weren't in US East 1 at all, they were in US East 2. If I only searched one region I would've thought nothing happened. Hackers use this too by working in regions nobody checks, which is why my trail covers every region.

---

### November 3, 2026: multiregion split

**How this fits in the real world**
- Failed vs successful login: I compared my failed and successful logins side by side. Same IP, same browser, a minute apart, so it was just a wrong password, not an attack. Analysts make this call all day. One failed login is normal, but 20 from a foreign IP is not.
- The code: The script I found loops through every AWS region and checks each one for ConsoleLogin events, so I didn't have to click through 17 regions by hand. I made sure I understood what it did before running it, because running code you don't understand is a real security risk. In the real world, analysts use scripts like this to search huge amounts of logs fast.
- CloudWatch Logs: I connected my trail to CloudWatch so I can set up alerts next. Saving logs is only half the job, because in the real world you want to know something is happening right away, not weeks later.

### November 5, 2026: S3 URI

**How this fits in the real world**
- ### Reflection

- I thought my login was in the wrong region, but the logs only proved I opened the wrong file. In this situation it taught me to only report what the evidence actually shows. In real companies, a wrong conclusion can get an innocent employee locked out or let a real attacker slip by.
- The only event in my first file was GetBucketAcl, which was just AWS checking its own bucket, not a person. In this case I learned that a lot of log activity is background noise. SOC analysts deal with thousands of events like this a day, so knowing what normal looks like stops them from wasting hours on false alarms.
- When I searched every region and found no login, it felt like I failed. But at a real company, a missing login could mean there's a gap in the logging, which means the business can't prove what happened or pass audits. That's why my next step is a controlled sign-in test to prove the trail actually works.

## November 6, 2026: Cloudtrail check
-### Reflection

-In this situation, where I had to test out all possible ways my lab went wrong, I learned the importance of why cybersecurity professionals urge the importance of troubleshooting your work. But I also learned to test a theory before reporting it as the answer. In real companies, an analyst who reports a guess as the root cause can send the whole team chasing the wrong problem while the real issue stays open.
- Before searching my logs again, I checked the trail’s health first: Logging was True, there were no delivery errors, and logs had been delivered just minutes before. In this case I learned that “I found nothing” only means something if you know the tool is working. In real companies, broken setups like mine can still be running dangerously in companies hiding their system flaws.
- I also compared what my trail saved against Event history, which keeps 90 days of activity even without a trail. In this case it showed me there are two records of activity and that my trail only covers events from when it started. In real companies, auditors and investigators would ask exactly when logging began, because anything before that has no permanent record. 

## November 10, 2026: cloud watch touch up
-### Reflection

In this session where I set up a metric filter to catch failed console logins, I learned that collecting logs and actually watching them are two different jobs. My trail was already saving every sign-in but nobody gets warned about a log that just sits there. In real companies a team can have perfect logging and still miss an attack because nothing is set up to flag the important events.

- Before building the alarm I had to create a metric filter that scans my CloudTrail logs for ConsoleLogin events with the error message Failed authentication, and adds 1 to a count every time it finds one. In this case I learned that a CloudWatch alarm can't read log text but it can only watch numbers, so the filter is what turns a log into something an alarm can react to. In real companies, security teams build these filters for the events they care about most because there's way too much log data for a person to read.
- I also learned how the filter pattern works: it only counts an event when both parts are true, the event has to be a console login and it has to have failed. In this case it showed me that a pattern that's too loose would count every normal login and spam me with alerts, and one that's too strict would miss attacks. In real companies analysts touch up their detections so they catch real threats without bombarding the team in false alarms, because a team that gets too many alerts starts ignoring them.

## November 11, 2026: Root account usage alarm

### Reflection

In this session I built a second alarm but this time for anyone using the root account. Since I already had the log group and email topic from yesterday, all I needed was a new filter and alarm. In real companies, once the logging and alerting setup is in place, adding a new detection is fast, which is why teams put in the work to build that setup first.

- Unlike yesterday's filter, this one doesn't care if a login worked or failed. It catches anything done as root. In this case I learned that since I stopped using root back in Phase 1, any root activity at all is a red flag, either it was me on purpose or someone has my password. In real companies, root use gets alerted every single time because root can't be limited by IAM policies, so it's the one account an attacker wants most.
- The filter also skips actions that AWS services do on root's behalf, so only a real person using root sets it off. In this case it showed me that even a "catch everything" alert still needsto cut out noise. In real companies, a detection that fires on background events gets ignored, and then it's useless when a real attack happens.

## November 12, 2026: Testing both alarms

### Reflection

In this session I didn't build anything new. I tested both of my alarms, the failed-login one and the root one, by setting them off myself back to back. Before today I only thought they worked. Now I have the emails to prove it. In real companies, teams test their own detections on purpose, because an alert that's never been tested is just a guess.

- I did a failed login as my IAM user and then signed in as root, and each alarm sent me its own email. In this case I learned that a good test doesn't just check that something fired, it checks that the right alarm fired for the right event and that neither one mixed them up. In real companies, detections that overlap or fire on the wrong thing confuse whoever's responding, and they waste time chasing the wrong problem.
- I used the alarm emails as my proof instead of the CloudWatch alarm page, because an email only shows up if every step worked, from the login to the log to the filter to the alarm to my inbox. In this case it showed me that the best evidence proves the whole chain worked, not just one piece of it. In real companies, teams test the whole path from the event to the alert, because a single problem means the alert never reaches anyone.
