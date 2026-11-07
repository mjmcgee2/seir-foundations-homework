SEIR Foundations – Lab 0: EC2 Basics
This lab demonstrates launching EC2 instances using a Launch Template, connecting via EC2 Instance Connect, verifying the SEIR dashboard, and terminating instances. Three deployments were completed as required.

===============================================================================

| Deployment | Public IPv4 Address | Screenshot File | Notes |
| --- | --- | --- | --- |
| **1** | **3.138.61.255** | ``screenshots/deployment-1.png`` | Successful connection; dashboard displayed |
| **2** | **18.221.169.234** | ``screenshots/deployment-2.png`` | Successful connection; dashboard displayed |
| **3** | **3.148.211.131** | ``screenshots/deployment-3.png`` | Successful connection; dashboard displayed |

===============================================================================

seir-foundations-homework/module1/lab0_basics/screenshots/

Files:

deployment-1.png

deployment-2.png

deployment-3.png

Each screenshot includes:

EC2 Instance Connect terminal

SEIR dashboard

Public IPv4 address recorded in this README (per assignment guidance)

===============================================================================

What I Did
I created an EC2 Launch Template using the AMI and User Data provided in the SEIR Foundations instructions. I launched three separate EC2 instances from the template, waited for each to pass 2/2 status checks, and connected using EC2 Instance Connect. After connecting, I verified that the SEIR dashboard appeared, confirming that the User Data executed correctly.

What I Learned
I learned how Launch Templates simplify repeatable EC2 deployments, how EC2 Instance Connect works without SSH keys, and how User Data scripts run during instance initialization. I also learned how to properly capture evidence, organize screenshots, and structure a GitHub submission.

Issues Encountered
My main challenge was understanding GitHub folder structure and how to organize the homework repository. I resolved this by creating the required folders locally and pushing them to GitHub. I also clarified the screenshot requirements and confirmed that including the public IPv4 in the README satisfies the assignment guidance.

✔️ Lab Completed
All three deployments were launched, connected, verified, documented, and terminated successfully.