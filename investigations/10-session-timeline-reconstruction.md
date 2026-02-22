# Lab 10 — Session Timeline Reconstruction

## Objective
Reconstruct a timeline of failed SSH login attempts by extracting timestamps from Linux authentication logs to evaluate whether the activity appears manual or automated.

## Log Source
/var/log/auth.log


## Step 1 — Filter Failed Login Attempts
Command:
grep -a "Failed password" /var/log/auth.log

Purpose:
Filters authentication logs to show only failed password events.


## Step 2 — Extract Timestamps
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $1}'

Purpose:
Extracts the timestamp field (field $1) from each failed login event to rebuild the event sequence.


## Findings
Output:
2025-10-06T14:04:24.568432-04:00
2025-10-06T14:04:29.015654-04:00
2025-10-06T14:04:33.199329-04:00
2025-10-06T14:04:44.026617-04:00


## Timeline Gap Analysis
Approximate gaps between attempts:

- Attempt 1 → 2: ~5 seconds 
- Attempt 2 → 3: ~4 seconds 
- Attempt 3 → 4: ~11 seconds 


## Interpretation
- Events are seconds apart and not perfectly uniform.
- This timing pattern is more consistent with manual attempts than automated brute force (which often produces rapid or highly consistent intervals).

---

## Conclusion
The failed authentication events form a short session timeline with irregular spacing, indicating a likely manual attempt sequence rather than automation.
