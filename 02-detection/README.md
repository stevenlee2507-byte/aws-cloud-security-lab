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


