## Senior Analyst Briefing

"Last night, our Application Server underwent scheduled maintenance. A few hours after the maintenance was completed, our SIEM generated a **High Severity Alert** early this morning.

According to the alert, it appears that a new privileged user account has been created on the server.

At this stage, we don't know whether this account was created as part of an approved maintenance activity or by an attacker who gained unauthorized access and established persistence.

Do not jump to conclusions. Every finding must be supported by evidence. No assumptions should be made until the investigation is complete.

Your assignment is to perform a comprehensive investigation of the server, collect all relevant evidence, and document every observation throughout the process.

Pay special attention to the following:

* Verify the server's basic information and overall health status.
* Identify all currently logged-in users.
* Determine whether any user accounts were created recently.
* Check the privilege level assigned to newly created accounts.
* Investigate whether any sensitive system files have been accessed.
* Search for hidden files or other suspicious artifacts that may indicate malicious activity.
* Review the authentication logs for any events related to account creation, privilege escalation, or unauthorized access.

Once your investigation is complete, prepare an incident summary along with recommendations based solely on the evidence you have collected.

Remember: A SOC Analyst reaches conclusions based on evidence—not assumptions.

Good luck."
