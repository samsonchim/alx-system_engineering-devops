Postmortem: Service Outage Due to Database Lock Contention

Issue Summary:

Duration: The outage lasted for 2 hours, from 10:00 AM to 12:00 PM WAT on August 15, 2024.
Impact: During the outage, our e-commerce platform experienced significant slowdowns, with page load times increasing by up to 10x. Approximately 70% of users were affected, with some unable to complete purchases or access product listings.
Root Cause: The root cause was a database lock contention issue caused by a long-running batch update process that locked several key tables, blocking other critical queries from executing.
Timeline:

10:00 AM: Issue detected by the monitoring system, which alerted the on-call engineer to a spike in response times and an increase in error rates.
10:05 AM: On-call engineer begins investigation, suspecting a potential network issue due to similar symptoms observed in the past.
10:15 AM: Network infrastructure is checked, but no abnormalities are found. The issue is escalated to the database team.
10:25 AM: Database team reviews logs and metrics, noticing high CPU usage and a large number of waiting queries.
10:40 AM: Initial assumption is that a specific database query may have regressed after a recent deployment, triggering further investigation into recent code changes.
11:00 AM: Misleading path identified as code review shows no anomalies. Attention shifts back to database performance metrics.
11:15 AM: Database lock contention is identified as the root cause, linked to a batch update process that started at 9:55 AM.
11:25 AM: The batch process is terminated, and database performance begins to recover.
12:00 PM: All services return to normal operation. A post-incident analysis meeting is scheduled.
Root Cause and Resolution:

Root Cause: The issue was caused by a batch update process that was initiated to update pricing information across multiple product categories. The process inadvertently locked several high-traffic tables, leading to a backlog of waiting queries. As the number of waiting queries grew, the database server became overwhelmed, causing significant delays and timeouts for user-facing services.

Resolution: The batch update process was immediately terminated, which released the locks and allowed the pending queries to execute. The affected tables were analyzed to ensure no data corruption occurred, and the system was monitored closely for any residual impact.

Corrective and Preventative Measures:

Improvements:

Implement better scheduling and throttling for batch processes to avoid peak traffic times.
Enhance database monitoring to detect lock contention earlier.
Improve communication between the development and operations teams to ensure awareness of potentially disruptive processes.
Tasks:

 Add a lock contention monitoring alert in the database monitoring system.
 Refactor the batch update process to use smaller transactions or partitioned updates.
 Conduct training for the engineering team on best practices for database updates.
 Review and document the incident response process to include database lock issues.
This incident highlighted the need for more robust processes around database management, particularly during large-scale updates. By implementing the corrective actions outlined above, we aim to prevent similar outages in the future and improve overall system resilience.
