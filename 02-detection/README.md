# Phase 2: Detection

Phase 1 locked the account down. Phase 2 makes sure that if something goes wrong anyway, I can see it. 

## 1. CloudTrail 

11/1/26 10:20PM

Before setting up the trail, I looked at CloudTrail's Event History. It already showed my Phase 1 work (`CreateUser`, `CreateGroup`,
`UpdateAccountPasswordPolicy`). But Event history only keeps 90 days and can't be changed, so I created a trail to keep a copy of the logs in
storage I control.

However, today a problem occurred during my testing of alerts. I logged in with the correct IAM user but not the correct password twice to trigger false login alerts within my console. However, after I did that and logged into my console with the correct credentials, I checked my console login event history but found none of the correct signals I was supposed to receive, to let me know my console successfully sends login alerts. Tomorrow, I will continue with this investigation to find out why.

![Trail settings](screenshots/01-trail-settings.png)
![Log files in S3](screenshots/02-s3-log-files.png)
![Event history results](screenshots/01-unsuccessful-investigation.png) 

## 2. Cloudtrail region investigation

11/2/26 1:09PM

Ran this code i found for cloudshell to investigate the failed investigation on my failed login. After investigating I found that the issue lay in the region of my aws console. 

- code: checks every region for consolelogin events. After running it in cloudshell, I discovered my IAM logins to be all hidden in the US east 2 region. So unlike my previous premonition where I thought I couldn't find my event history due to my event logs just not showing anything for IAM logins it was more just of a region issue. 
- cloudtrail: finally found the failed login attempts at November 1 9:13 pm after switching region to US east 2
- cloudtrail: also found my successful login at November 1 9:14 pm which upon analyzing both the successful and unsuccessful login attempts' JSONs, there were obvious differences.

![Region split](screenshots/04-consolelogin-region-split.png)
![Login event](screenshots/05-failed-login-event.png)
![Successful login](screenshots/06-successful-mfa-login.png)
