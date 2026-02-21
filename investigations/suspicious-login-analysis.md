## Objective
Detect possible brute-force compromise by analyzing authentication logs.

## Commands Used

Count failed logins:
grep -a "Failed password" /var/log/auth.log | wc -l

Identify top IP:
grep -a "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

Check for success:
grep "::1" /var/log/auth.log | grep "Accepted password"

## Findings
Failed attempts: 4 
Suspicious IP: ::1 
Successful login: None 

## Analysis
Multiple failed login attempts were detected from localhost (::1). No successful login occurred, indicating a failed brute-force attempt rather than a confirmed compromise.

## Verdict
No incident. Attack attempt only.

## Skills Demonstrated
- Log parsing
- Threat investigation
- Authentication analysis
- Command-line analysis
