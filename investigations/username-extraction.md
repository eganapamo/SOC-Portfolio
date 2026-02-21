## Lab - 3 Username Extraction

## Objective
Identify which usernames attackers attempted to log into by extracting the username field from failed SSH login attempts.


## Step 1 — Filter Failed Login Attempts
grep "Failed password" /var/log/auth.log


## Step 2 — Determine Username Field Position
grep "Failed password" /var/log/auth.log | head -1

# Manually count fields
# Example structure:
# Oct 10 04:04:24 kali sshd[64162]: Failed password for invalid user fakeuser from ::1 port 60870 ssh2

# Field breakdown example:
# 1=Oct
# 2=10
# 3=04:04:24
# 4=kali
# 5=sshd[64162]:
# 6=Failed
# 7=password
# 8=for
# 9=invalid
# 10=user
# 11=fakeuser

# Username field = $11


## Step 3 — Extract Only Usernames
grep "Failed password" /var/log/auth.log | awk '{print $11}'


## Step 4 — Count Username Attempts
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr


## Findings
Example output:
3 fakeuser


## Interpretation
• Attackers attempted login using username "fakeuser" 
• Repeated attempts indicate brute-force or enumeration behavior 
• Username targeting suggests attacker is testing common or guessed accounts


## Analyst Conclusion
System shows evidence of username enumeration activity.
Attacker is attempting to identify valid accounts before password attack phase.


## Skills Practiced
• Log analysis 
• Field extraction 
• Pattern recognition 
• Attack behavior identification 
• Bash pipelines 
