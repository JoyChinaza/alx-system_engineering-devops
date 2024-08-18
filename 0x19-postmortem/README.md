Postmortem: Web Application Outage on July 14, 2024
Issue Summary:

Duration: July 14, 2024, 10:15 AM - 12:30 PM UTC (2 hours, 15 minutes).
Impact: The web application was completely inaccessible to 90% of users. Affected users were unable to log in or access any services, resulting in a total service disruption for the majority of the user base.
Root Cause: A misconfiguration in the load balancer that caused a bottleneck, preventing traffic from being correctly routed to the backend servers.

Timeline:

10:20 AM: Issue detected via monitoring alerts indicating an unusual spike in 502 Bad Gateway errors across multiple services.
10:25 AM: On-call engineer receives alert and begins investigation, initially suspecting a database connectivity issue due to similar past incidents.
10:30 AM: Engineer checks database logs and connections; no issues found. The problem persists, and user complaints start appearing on social media.
10:40 AM: Network team is brought in to investigate possible network-related issues, focusing on firewall and DNS configurations.
11:00 AM: Investigation reveals no anomalies in firewall or DNS settings, but further probing shows traffic is not reaching the backend servers as expected.
11:15 AM: Misleading path explored: Application code is checked for potential bugs or recent deployments that could have introduced the issue. No new code deployments were made recently.
11:30 AM: Load balancer configuration is reviewed. It is discovered that a recent configuration change introduced a rule that inadvertently throttled incoming traffic.
11:45 AM: Load balancer configuration is rolled back to the previous version.
12:00 PM: Monitoring tools confirm that traffic flow is normalizing, and the application begins to recover.
12:30 PM: Full service is restored; incident is resolved and a post-incident review is scheduled.
Root Cause and Resolution:

Root Cause: The issue was caused by a misconfiguration in the load balancer. A recent configuration change added a rule intended to improve traffic distribution but inadvertently caused the load balancer to throttle traffic excessively. This resulted in the majority of requests not being forwarded to the backend servers, leading to the application being unavailable to most users.
Resolution: The issue was resolved by rolling back the load balancer configuration to the previous, stable version. Once the rollback was completed, traffic flow resumed normal operation, and the application became accessible again.

Corrective and Preventative Measures:

Improvements/Fixes:

Implement stricter review processes for load balancer configuration changes.
Increase redundancy in traffic routing to prevent single points of failure.
Enhance monitoring to detect load balancer configuration issues more quickly.
TODO:

Task 1: Patch the load balancer to include more granular logging for configuration changes.
Task 2: Add monitoring on the load balancer to detect and alert on unusual traffic patterns or throttling.
Task 3: Conduct training sessions for the engineering team on the impact of load balancer configurations.
Task 4: Review and update the rollback procedures to ensure faster response times in similar future incidents.

