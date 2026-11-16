SEIR Foundations - Module 1, Lesson 3
Jira + Slack Incident Integration Notes
BAM Integration Explanation
The Balerica AI incident workflow uses incident.py to coordinate Jira incident creation and Slack notification. When the integrated test runs, incident.py calls the Jira integration first so that Jira can create the incident record and return an issue key and URL. During my test, Jira successfully created issue BAIM-4 in the Balerica AI Incident Management project. After Jira created the issue, incident.py passed the Jira information to slack.py, which sent the incident alert to my #balerica-alerts channel. Jira is called before Slack because the Slack notification needs the newly created Jira issue URL so the engineer can follow the alert directly to the incident record. I verified the Jira side by opening BAIM-4 and confirming that the issue contained the same incident ID and simulated application failure reported by the integration test. I then verified the Slack side by confirming that the matching alert appeared in #balerica-alerts and that its Jira link opened the correct BAIM-4 issue.

Troubleshooting Note
While validating the Jira configuration, my safe environment-variable check reported JIRA_EMAIL as missing even though I had already entered the email information in .env. I found that I had accidentally typed the variable name as JJIRA_EMAIL instead of JIRA_EMAIL. After correcting the variable name, I reran the configuration check and all required variables reported as set. I then ran python3 jira.py --verify, which successfully connected to the BAIM Balerica AI Incident Management project. This reinforced that environment-variable names must match exactly what the application expects.

Integration Verification
Slack standalone test: successful and confirmed in #balerica-alerts
Jira verification: successful
Jira project: BAIM - Balerica AI Incident Management
Integration dry run: successful; no external API calls made
Integrated test Jira issue: BAIM-4
Jira issue creation: successful
Slack delivery: successful
Slack Jira link: verified to open BAIM-4


BAM 5.6 - Duplicate Detection
The duplicate-detection test used the stable incident ID BAM-DUP-MJM-001. The incident integration stores incident state in the local SQLite database at data/incident_state.db, allowing incident.py to recognize an incident ID that has already been processed. The first occurrence created Jira issue BAIM-5, while later executions using the same incident ID returned the same BAIM-5 issue instead of creating another Jira ticket. The repeat executions reported jira_created: False and slack_status: already_sent, demonstrating that the incident had already been processed. If a different incident ID were used, the integration would treat it as a different incident and could create a new Jira issue.
