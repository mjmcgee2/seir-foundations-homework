Lesson 2 Homework - Seven Alerts, Seven Wins
Homework 1 - Hello Slack
I edited MY_NAME in hw1_hello_slack.py and ran:

python hw1_hello_slack.py

The script successfully delivered my customized INFO alert to #balerica-alerts with incident ID HW-001.

Terminal result:

Delivered! Go look at #balerica-alerts. That message is yours.

The Slack message confirmed that my EC2 instance could call send_alert() from slack.py and deliver a customized message through the Slack webhook.

Homework 2 - Severity Tour
Question: Which severities arrived, and why those?

INFO, WARNING, ERROR, and CRITICAL all arrived in Slack. My alert threshold was set to INFO in .env, so INFO was the minimum severity allowed through and every severity at or above INFO was delivered.

Terminal result:

INFO -> DELIVERED to Slack
WARNING -> DELIVERED to Slack
ERROR -> DELIVERED to Slack
CRITICAL -> DELIVERED to Slack

Homework 3 - Server Time
Question: How many hours apart are server time and my wall clock?

The EC2 server time was 5 hours ahead of my local time. The server reported 01:47:05 UTC while my local time in Massachusetts was approximately 8:47 PM EST. This demonstrated why using UTC on servers provides a consistent time reference that does not depend on the administrator's local time zone.

Terminal result:

Local fact: the server believes it is Tuesday November 10, 2026 at 01:47:05 UTC.

Homework 4 - Disk Watch
Question: Why is a quiet INFO here GOOD design, not a failure?

My EC2 disk was only 25.3% used, so the script correctly classified the condition as INFO rather than WARNING or ERROR. This is good monitoring design because a healthy system should not generate unnecessary high-severity alerts. In an operations environment, generating warnings for normal conditions creates noise and could make genuinely important alerts easier to miss.

Terminal result:

Disk is 25.3% used -> severity INFO.

Homework 5 - Load Check
Question: What would make this number spike on the Balerica bank?

The load average could spike if the Balerica server suddenly had much more work to process, such as many API requests, transaction-processing jobs, or other CPU-intensive work occurring at the same time. My EC2 reported load averages of 0.00 / 0.00 / 0.00 on 2 CPUs, so the server was essentially idle during the test. The script is designed to classify the condition as WARNING if the sustained 5-minute load exceeds the number of available CPUs.

Terminal result:

Load: 0.00 / 0.00 / 0.00 on 2 CPU(s) -> INFO

Homework 6 - Incident Drill
I ran hw6_incident_drill.py three times as instructed.

The three randomly selected incidents were:

DRILL-9869 - ERROR - payment_post returned code 8: business rejection rate above 5%.
DRILL-4723 - WARNING - SWIFT traffic generator queue depth climbing: 42 messages waiting.
DRILL-4314 - WARNING - SWIFT traffic generator queue depth climbing: 42 messages waiting.
All three were delivered because my alert threshold was set to INFO.

Question: Which of these fake incidents deserves to wake a human at 3 AM?

The CRITICAL incident where account_lookup returns code 12 because the ledger file is not reachable is the condition I would consider worthy of waking someone at 3 AM. I learned in Lesson 1 that return code 12 represents an environment or file-access failure. If the banking application cannot access its ledger, that could prevent important banking functions from operating. WARNING conditions should still be investigated, but they do not necessarily indicate an immediate outage requiring someone to be awakened.

Homework 7 - Morning Report
I ran hw7_morning_report.py, which combined several system checks into a single monitoring report.

My report contained:

UTC timestamp
EC2 hostname and operating system
Python version
Disk utilization
1-minute load average
Overall status
The script reported:

Disk on / : 25.3% used
Load (1m): 0.00
Status: all checks ran. The bank greets the day.

The report was successfully delivered to Slack as incident HW-007. I also captured the required terminal screenshot showing the Lesson 1 ID line together with the morning-report execution.

Experiments
Experiment 1 - Severity Threshold
Prediction:

After changing SLACK_ALERT_SEVERITY from INFO to CRITICAL, I predicted that only one of the four alerts from hw2_severity_tour.py would reach Slack: the CRITICAL alert.

Result:

My prediction was correct.

INFO -> skipped by your threshold
WARNING -> skipped by your threshold
ERROR -> skipped by your threshold
CRITICAL -> DELIVERED to Slack

What I learned:

The severity setting in .env acts as a minimum alert threshold. Changing configuration changed the behavior of the alerting system without requiring any modification to the Python code.

Experiment 2 - Alert Kill Switch
Prediction:

I predicted that no alert would be sent when I changed:

SLACK_ALERTS_ENABLED=false

Result:

The script reported:

Skipped. Check .env: SLACK_ALERTS_ENABLED must be true and SLACK_ALERT_SEVERITY=INFO (see homework README).

No new HW-001 alert was delivered to Slack.

What I learned:

SLACK_ALERTS_ENABLED acts as a kill switch for alert delivery. This would be useful during planned maintenance because systems may intentionally be stopped, restarted, or temporarily unavailable. Disabling notifications during that window can prevent expected maintenance conditions from producing false-positive or unnecessary alerts.

Experiment 3 - Disk Warning Threshold
My original disk usage was approximately 25.3%, which produced an INFO alert with the original 75% WARNING threshold.

Prediction:

After lowering the WARNING threshold from 75% to 20%, I expected the same 25.3% disk usage to be classified as WARNING instead of INFO.

Result:

My prediction was correct.

Disk is 25.3% used -> severity WARNING.

The WARNING alert was delivered to Slack.

What I learned:

The actual disk condition had not changed; the monitoring threshold used to interpret that condition had changed. This demonstrated how monitoring policy can change alert behavior without any change to the underlying server condition.

Experiment 4 - Restore Configuration
After completing the experiments, I restored the environment to the intended homework configuration:

SLACK_ALERTS_ENABLED=true
SLACK_ALERT_SEVERITY=INFO

I also restored the disk WARNING threshold in hw4_disk_watch.py from the experimental value of 20 back to its original value of 75.

Final Takeaway
This homework helped connect basic Python scripting with concepts I already understand from IT operations. Instead of simply generating messages, the later scripts measured actual server conditions, compared those conditions against thresholds, selected an appropriate severity, and sent the resulting alert to Slack. The experiments also showed that good monitoring depends not only on collecting data, but on choosing sensible thresholds and controlling when alerts should or should not reach the operations team.