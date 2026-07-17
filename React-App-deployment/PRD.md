Assignment: Production Maintenance Drill (OPS Checklist)


1. Assignment Overview
Assignment: Production Maintenance Drill (OPS Checklist)
Estimated Time: 90 minutes
Difficulty: Advanced
Category: AWS / Linux / DevOps / Production Operations



2. Objective
In this assignment, you will treat your already deployed React application (on Ubuntu VM with Nginx) as a production system.

You will perform structured operational checks including:

Network validation

Service health checks

Log analysis

System resource monitoring

Configuration verification

Incident simulation and recovery

This helps you understand real-world on-call DevOps responsibilities.



3. Real-World Scenario
Your React application is already deployed on an Ubuntu EC2 instance using Nginx.

In real production environments (like EpicReads or similar systems), deployments are not the end of work. Engineers must continuously:

Monitor system health

Validate traffic flow

Check logs for hidden issues

Respond to incidents

Recover services safely

As a DevOps Engineer, you are now acting as an on-call engineer responsible for production stability.



4. Learning Outcomes
By the end of this assignment, you will be able to:

Perform production-grade health checks on Linux servers

Validate network connectivity and service exposure

Analyze Nginx logs for real traffic behavior

Monitor CPU, memory, and disk usage

Verify deployment integrity and configuration safety

Simulate and recover from production incidents

Think like an on-call DevOps engineer



5. Important Instructions (Global Rules)
Follow the Assignment Submission Guidelines Click here

Key Rules:
Full Name must be visible in required screenshots

Do not expose sensitive information (keys, passwords, account IDs)

Follow screenshot requirements exactly as specified in tasks

Submission must clearly match task outputs

Missing or incorrect proof may result in rejection



6. Prerequisites
Required
Ubuntu EC2 instance (from previous assignment)

React application already deployed with Nginx

Basic Linux command knowledge

AWS Free Tier account

System Assumptions
Your VM should already have:

Node.js & npm installed

Nginx installed and running

React build deployed to /var/www/html

Public IP accessible via browser



7. Tasks
Each task must be completed sequentially.



Task 1 — Server Access & Networking Validation
Goal
Verify that the deployed React application is reachable from the browser and confirm basic network connectivity of the Ubuntu VM.



Steps
Open the React application in a browser using the public IP address

Check network interfaces and assigned IP addresses

Check routing table (default gateway configuration)

Validate DNS resolution

Test external network connectivity

Check open network ports and associated processes

Check firewall status



Commands
N/A

ip a

ip route

dig pravinmishra.com +short OR host pravinmishra.com

ping -c 4 thecloudadvisory.com

sudo ss -tulpen

sudo ufw status



Expected Outputs
Step 1: React application loads successfully at http://<public-ip> in browser

Step 4: DNS resolution returns a valid IP address or response

Step 6: Port 80 (HTTP/Nginx) and 22 (SSH) should be visible and in listening state



Screenshots Required
Screenshot 1: Browser showing React app + your Full Name on UI

Screenshot 2: Output of ip a

Screenshot 3: Output of sudo ss -tulpen

Screenshot 4: Output of sudo ufw status



Notes You Must Write (Very Important)
Answer the following:

What proves Nginx is listening on 0.0.0.0:80?

What proves SSH is active on port 22?

Did you find any unexpected open ports? Explain briefly.



Task 2 — Service Health & Systemd Validation (Nginx)
Goal
Verify that Nginx is properly installed, running, enabled at boot, and safely configured.



Steps
Check Nginx service status

Check if service is enabled on boot

Validate Nginx configuration

Check running Nginx processes

Confirm which PID is listening on port 80:

Perform safe restart test

Check Nginx service status again



Commands
systemctl status nginx --no-pager

systemctl is-enabled nginx

sudo nginx -t

ps aux | grep -E "nginx: master|nginx: worker" | grep -v grep

sudo ss -lptn '( sport = :80 )'

sudo systemctl restart nginx

systemctl status nginx --no-pager



Expected Outputs
Command 1: Active (running)

Command 2: enabled

Command 3: syntax ok + test successful

Command 5: nginx bound to port 80



Screenshots Required
Screenshot 1: Output of systemctl status nginx --no-pager(Command 1)

Screenshot 2: Output of  sudo nginx -t

Screenshot 3: Output of  sudo ss -lptn '( sport = :80 )'



Notes You Must Write (Very Important)
Answer the following:

What happens if Nginx fails to restart in production?

What’s your basic rollback plan?



Task 3 — Logs & Request Trace
Goal
Verify real traffic flow and analyze logs to understand system behavior and errors.

In industry, “it’s working” is not enough. We check logs to confirm clean traffic and find hidden errors.



Steps
Generate traffic

Check Response Status

Check access logs

Check error logs

Check system logs



Commands
curl -s http://<public-ip> /dev/null

curl -I http://<public-ip> /dev/null

sudo tail -n 30 /var/log/nginx/access.log

sudo tail -n 30 /var/log/nginx/error.log

sudo journalctl -u nginx --no-pager -n 50



Expected Outputs
Command 1: No terminal output

Command 2: HTTP status shown (e.g., 200 OK)

Command 3: Recent HTTP requests visible (including your curl request)

Command 4: No critical errors present (or only minimal warnings)

Command 5: Nginx startup/service logs visible



Screenshots Required
Screenshot 1: Output of sudo tail -n 30 /var/log/nginx/access.log

Screenshot 2: Output of sudo tail -n 30 /var/log/nginx/error.log

Screenshot 3: Output of sudo journalctl -u nginx --no-pager -n 50



Notes You Must Write (Very Important)
Answer the following:

1. Were there any errors in the logs?

If yes, mention 1–2 example error lines from the logs and explain what each one means in simple terms.

2. If there were no errors, what does that indicate about the system?

Explain what it means if the error log is empty or shows no recent errors during your check.

Hint:
This does NOT mean the system is permanently perfect.
It only means no issues were recorded during the time period you analyzed.



3. Based on the access logs, were your curl requests visible in the log entries?

Check the access log output.

Confirm whether your curl requests appear there or not.

Briefly explain what that proves about traffic flow.



Task 4 — System Resource Health Check (Capacity Red Flags)
Goal
Assess server capacity and detect potential performance or failure risks.

In simple terms: Can this server handle traffic? Or is it close to breaking?



Steps
Check uptime

Check memory usage

Check disk usage

Identify large directories inside /var



Commands
uptime

free -h

df -h

sudo du -sh /var/* | sort -h



Expected Outputs
System running duration is displayed ( Shows how long the server has been up without restart )

Total memory, used memory, and available memory are shown

Disk usage shown in percentage (%)

List of directories inside /var sorted by size ( Helps identify which folder is consuming the most space )



Screenshots Required
Screenshot 1: Output of uptime

Screenshot 2: Output of free -h

Screenshot 3: Output of df -h

Screenshot 4: Output of sudo du -sh /var/* | sort -h



Notes You Must Write (Very Important)
Answer the following:

Which resource looks most critical right now?

Check your outputs and identify which one is most concerning:

CPU / load (from uptime)

Memory usage (from free -h)

Disk usage (from df -h)

Explain why you think it is important.



What happens if disk becomes 100% full in a production server?

Explain in simple terms what breaks when disk is full.

Hint:

Logs may stop writing

Applications may fail

Server may become unstable or unresponsive



Task 5 — Configuration & Deployment Verification
Goal
Ensure the correct React build is deployed and Nginx is serving it properly.

In simple terms: Are we serving the correct frontend version through Nginx?



Steps
Web Root File Check

Deployment Verification (Your Name Check)

SPA Routing Configuration Check



Commands
ls -lah /var/www/html | head -n 20

grep -R "Deployed by" -n /var/www/html 2>/dev/null | head

grep -n "try_files" /etc/nginx/sites-available/default



Expected Outputs
A list of files and folders inside /var/www/html is displayed

A line should appear containing: "Deployed by <Your Name>"

This confirms:

React source code was updated correctly

Build includes your custom name

Nginx configuration should include SPA routing rule: try_files $uri /index.html;

This confirms:

React routing works properly on refresh

Nginx is configured for SPA deployment



Screenshots Required
Screenshot 1: Output of ls -lah /var/www/html | head -n 20

Screenshot 2: Output of grep -R "Deployed by" -n /var/www/html 2>/dev/null | head

Screenshot 3: Output of grep -n "try_files" /etc/nginx/sites-available/default



Notes You Must Write (Very Important)
Answer the following:

How do you confirm that the correct version of the application is deployed?

What you should explain in your answer:

Write how you verified the deployment using the following points:

You checked the files in /var/www/html using ls -lah

You confirmed the React build files are present (like index.html and static folder)

You verified your custom change is deployed by checking the "Deployed by <Your Name>" line in the code

You ensured Nginx is serving the correct application through the correct web root

You validated that the application loads correctly in the browser



Task 6 — Nginx Configuration Failure Simulation
Goal
Simulate a real-world Nginx misconfiguration and recover the service safely.



Steps
Introduce a configuration error in Nginx

Open Nginx config file

Remove the semicolon ; from this line: try_files $uri /index.html;

Validate broken configuration

Fix the configuration

Open Nginx config file

Re-add the semicolon ; to this line: try_files $uri /index.html

Validate nginx configuration again

Restart the service

Confirm recovery



Commands
a. sudo nano /etc/nginx/sites-available/default

b. Remove the semicolon ; from this line: try_files $uri /index.html;

sudo nginx -t

a. sudo nano /etc/nginx/sites-available/default

b. Re-add the semicolon ; to this line: try_files $uri /index.html

sudo nginx -t

sudo systemctl restart nginx

curl -I http://<public-ip>



Expected Outputs
Command 2: FAIL (syntax error shown)

Command 4: OK (syntax is ok)

Command 6: returns 200 OK



Screenshots Required
Screenshot 1: Output of sudo nginx -t (Command 2)

Screenshot 2: Output of sudo nginx -t (Command 4)

Screenshot 3: Output of curl -I http://<public-ip> (Command 6)



Notes You Must Write (Very Important)
Answer the following:

What caused the configuration failure?

How did you fix the issue?

How can you avoid this kind of issue in real production systems?



Task 7 — Web Application Failure Simulation
Goal
Simulate missing deployment content and recover the application safely.



Steps
1. Backup the deployed web application directory (safe state) (To safely simulate failure without data loss)

2. Create an empty directory with the same name as the previous web directory (This simulates missing deployment content)

3. Confirm failure (Verify that the application is no longer serving the correct content)

4. Restore application from backup (Bring back the original deployed files)

5. Restart Nginx service (Apply changes and reload configuration)

6. Confirm recovery (Verify that the application is working again)



Commands
sudo mv /var/www/html /var/www/html_backup

sudo mkdir -p /var/www/html

curl -I http://<public-ip>

a. sudo rm -rf /var/www/html

b. sudo mv /var/www/html_backup /var/www/html

sudo systemctl restart nginx

curl -I http://<public-ip>



Expected Outputs
Command 3: request fails or returns non-200 response

Command 6: returns 200 OK or valid HTTP response



Screenshots Required
Screenshot 1: Output of curl -I http://<public-ip> (Command 3)

Screenshot 2: Output of curl -I http://<public-ip> (Command 6)



Notes You Must Write (Very Important)
Answer the following:

What caused the application to break in this scenario?
(Hint: missing or removed web content directory)

How did you fix the issue and restore the application?

What steps would you take to prevent this kind of issue in real production systems?



Task 8 — Security & Reliability Review
Goal
Review and reflect on the security and reliability practices applied during this assignment.



Steps
Review server security practices

Review service reliability practices

Document your observations



Commands
N/A



Expected Outputs
N/A



Screenshots Required
N/A



Notes You Must Write (Very Important)
Create a section titled Security & Reliability Notes and answer the following in your own words:

Why is SSH key-based authentication more secure than sharing passwords?

Why should only required ports be open on a production server?

Why is it important for Nginx to be enabled on boot?

What are the risks of sharing secrets, keys, or credentials publicly?

Why should cloud resources be stopped or terminated when they are no longer needed?



8. Industry Insight
In real-world DevOps environments, deployment is only the beginning. Engineers are responsible for continuously monitoring application health, validating service availability, analyzing logs, verifying system resources, and responding to incidents. Regular health checks and recovery drills help teams detect issues early, reduce downtime, and ensure production systems remain secure, reliable, and available to users. The skills practiced in this assignment mirror common on-call and production support responsibilities performed by DevOps and Site Reliability Engineering (SRE) teams.



9. Submission Instructions
Complete all tasks in sequence.

Your submission must include:

All required screenshots

Final working application URL

Submit only a Google Doc link.

Follow the Assignment Submission Guidelines: Click here



10. Solution Walkthrough (Solution + Error Handling)
A step-by-step solution and troubleshooting guide is available for reference:

Full solution walkthrough → Click here

Step by step solution → Click here

References → Click here

Error handling & troubleshooting → Click here



11. LinkedIn Requirement
Create a LinkedIn post including:

A brief summary of the production maintenance activities you performed in this assignment

3–5 lines about what you learned from the following tasks:

Task 3 — Logs & Traffic Analysis

Task 4 — System Resource Health Check (Capacity Red Flags)

Task 6 / Task 7 — Incident Simulation & Recovery

Screenshot from Task 1 — Output of sudo ss -tulpen (showing active services and open ports)

Screenshot from Task 6 or Task 7 — Output of curl -I http://<public-ip> confirming successful recovery (HTTP 200 or valid response)

Submit:

LinkedIn post URL

Screenshot of the published LinkedIn post



12. Completion Checklist
Before submission, verify:

All tasks completed

React app runs successfully

Full Name visible in application

Screenshots included

No sensitive data exposed

Google Doc is accessible

Link tested in incognito mode



13. Final Submission
Submit only your Google Doc link.

Questions for this assignment
Based on the instructions and tasks above, submit your completed document with all required explanations and screenshots.

