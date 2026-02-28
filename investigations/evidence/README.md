# Investigation Evidence

## Objective
Identify failed SSH login attempts and determine attacker patterns.

## Steps Performed

- Searched authentication logs for failed login attempts
- Extracted usernames involved in failed attempts
- Extracted source IP addresses
- Correlated usernames with IP addresses
- Calculated frequency of attempts

## Findings
- Multiple failed login attempts detected
- Username "fakeuser" targeted
- Source IP ::1 observed repeatedly
- Pattern indicates brute-force behavior

## Conclusion
Evidence suggests automated authentication attack activity.

## Evidence Files
- 01 → Log search results 
- 02 → Username extraction 
- 03 → IP extraction 
- 04 → Correlation results 
- 05 → Frequency analysis
