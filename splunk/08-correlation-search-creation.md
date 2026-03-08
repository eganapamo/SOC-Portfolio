# Lab 8 – Correlation Search Creation

## Objective
Create a correlation search in Splunk to detect suspicious authentication behavior by identifying multiple failed login attempts followed by successful logins from the same attacker IP. This helps detect brute-force attacks that result in successful compromise.

## Environment
- Splunk Enterprise
- Linux syslog authentication logs
- Metasploitable VM generating SSH activity
- Kali Linux used to simulate attacker login attempts


## Step 1 – Search Authentication Events

Begin by searching both failed and successful SSH login events.

Search:

index=main sourcetype=syslog ("accepted password" OR "failed password")

This limits the dataset to authentication activity.


## Step 2 – Extract Attacker IP Address

If Splunk does not automatically extract the source IP, use rex.

Search:

index=main sourcetype=syslog ("accepted password" OR "failed password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"

This creates the field:

attacker_ip


## Step 3 – Extract Username

Extract the username from the authentication logs. 
This regex handles both normal usernames and invalid user attempts.

Search:

index=main sourcetype=syslog ("accepted password" OR "failed password")
| rex "for\s+(invalid user\s+)?(?<user>\S+)"

This creates the field:

user


## Step 4 – Classify Authentication Status

Use eval to classify each event as either failed or success.

Search:

index=main sourcetype=syslog ("accepted password" OR "failed password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| eval status=if(searchmatch("failed password"),"failed","success")

This creates the field:

status

Possible values:

failed
success


## Step 5 – Correlate Failed and Successful Logins

Use stats to count both failed attempts and successful logins from the same attacker IP.

Search:

index=main sourcetype=syslog ("accepted password" OR "failed password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| eval status=if(searchmatch("failed password"),"failed","success")
| stats count(eval(status="failed")) as failed_attempts count(eval(status="success")) as success_login values(user) as usernames by attacker_ip host

This correlates:

- attacker IP
- usernames targeted
- target host
- failed attempts
- successful logins


## Step 6 – Add Detection Threshold

Use where after stats to filter suspicious results.

Search:

index=main sourcetype=syslog ("accepted password" OR "failed password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| eval status=if(searchmatch("failed password"),"failed","success")
| stats count(eval(status="failed")) as failed_attempts count(eval(status="success")) as success_login values(user) as usernames by attacker_ip host
| where failed_attempts > 5

This filters results to show only likely brute-force behavior.


## Step 7 – Clean Output

Rename fields for readability and present the final table.

Search:

index=main sourcetype=syslog ("accepted password" OR "failed password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| eval status=if(searchmatch("failed password"),"failed","success")
| stats count(eval(status="failed")) as failed_attempts count(eval(status="success")) as success_login values(user) as usernames by attacker_ip host
| where failed_attempts > 5
| rename attacker_ip as attacker host as target_host
| table attacker usernames target_host failed_attempts success_login

Example output:

attacker         usernames   target_host      failed_attempts   success_login
192.168.56.104   msfadmin    192.168.56.102   9                 4


## Analysis

This correlation search identified repeated failed login attempts followed by successful authentication from the same attacker IP. The results show that attacker IP 192.168.56.104 targeted user msfadmin on host 192.168.56.102, generating 9 failed attempts and 4 successful logins.

This behavior strongly suggests a brute-force attack that eventually succeeded.


## Conclusion

This lab demonstrated how correlation searches in Splunk can link multiple authentication events together to detect brute-force compromise. By combining field extraction, event classification, aggregation, and threshold filtering, analysts can create alerts that identify suspicious attacker behavior and reduce time to detection.
