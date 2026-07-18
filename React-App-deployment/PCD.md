 Assignment Overview
Assignment: Deploy EpicReads Portfolio Website via Nginx

Estimated Time: 45 minutes

Difficulty: Intermediate

Category: AWS / Linux



2. Objective
In this assignment, you will deploy a static portfolio website on an Ubuntu VM using Nginx.

This assignment helps you understand real-world Linux server deployment, static website hosting, and Nginx web server configuration.



3. Real-World Scenario
EpicReads wants to launch a simple marketing and portfolio website quickly.

As a Junior DevOps Engineer, your responsibilities are to:

Deploy the website correctly.

Ensure Nginx serves the website reliably.

Prove ownership by adding your deployment details.

Verify the website is publicly accessible through its public IP.



4. Learning Outcomes
By the end of this assignment, you will be able to:

Deploy a static website on an Ubuntu VM.

Manage website files on a Linux server.

Deploy static web content using Nginx.

Verify website deployment using browser testing.

Perform basic operational checks on an Nginx server.



5. Important Instructions (Global Rules)
Follow the Assignment Submission Guidelines. Click here

Key Rules
Full Name must be visible in required screenshots.

Do not expose sensitive information (keys, passwords, account IDs).

Ownership proof in the footer is mandatory.

Website must remain publicly accessible for 24 hours after deployment.

Do not stop or terminate the VM before verification.

Follow screenshot requirements exactly as specified.





6. Prerequisites
Required
Ubuntu VM (recommended: same VM from Assignment 3)

Nginx installed

Internet access on VM

HTTP (Port 80) allowed in the Security Group



7. Tasks
Complete each task sequentially.



Task 0 — Pre-flight Check
Goal
Verify the Ubuntu VM and Nginx are ready for deployment.



Steps
Check the hostname.

Verify the Ubuntu version.

Verify the installed Nginx version.

Check the Nginx service status.

Commands
hostname

lsb_release -a

nginx -v

sudo systemctl status nginx --no-pager



Expected Output
Hostname displayed.

Ubuntu distribution information displayed.

Nginx version displayed.

Nginx service shows Active (running).



Screenshots Required
Screenshot 0 – Output of sudo systemctl status nginx --no-pager showing active (running).



Task 1 — Get the Website Source Code
Goal
Download and extract the portfolio website template.



Steps
Download the website template.

Install unzip (if required).

Extract the ZIP file.

Verify the extracted files.



Commands
wget https://github.com/pravinmishraaws/Pravin-Mishra-Portfolio-Template/archive/refs/heads/main.zip -O site.zip

sudo apt install -y unzip

unzip -o site.zip

ls -la



Expected Output
Portfolio website folder extracted successfully.



Screenshots Required
Screenshot 1 – Output of ls -la showing the extracted project folder.



Task 2 — Add Ownership Proof (Anti-Copy Change)
Goal
Update the website footer with your deployment details.



Steps
Navigate to the project folder.

Locate the file containing the footer.

Open the file using Nano.

Add your deployment information.

Save and exit.

Verify the changes.



Commands
cd Pravin-Mishra-Portfolio-Template-main

ls

grep -rl "Crafted with"

nano index.html



Replace:

<p>Crafted with <span>cloud</span> excellence by Pravin Mishra</p>


Add:

<p><strong>Deployed by:</strong> DMI Cohort 3 | Your Full Name | your Group | Week 3 | DD-MM-YYYY</p>


Save:

CTRL + O

Press Enter

CTRL + X

Verify:

cat index.html | grep -A 3 "Crafted with"



Expected Output
Footer updated with:

Full Name

Group Number

Week

Date



Screenshots Required
Screenshot 2 – Edited footer visible in Nano showing your deployment details.



Task 3 — Deploy Website via Nginx
Goal
Deploy the portfolio website to the Nginx web root.



Steps
Remove the existing website files.

Copy the portfolio website files.

Set ownership.

Set permissions.

Test the Nginx configuration.

Restart Nginx.

Verify the deployed files.



Commands
sudo rm -rf /var/www/html/*

sudo cp -r ~/Pravin-Mishra-Portfolio-Template-main/* /var/www/html/

sudo chown -R www-data:www-data /var/www/html

sudo chmod -R 755 /var/www/html

sudo nginx -t

sudo systemctl restart nginx

ls /var/www/html



Expected Output
Website files copied successfully.

Nginx configuration test successful.

Website files visible in /var/www/html.



Screenshots Required
Screenshot 3 – Output of sudo nginx -t

Screenshot 4 – Output of ls /var/www/html



Task 4 — Verify Website is Live
Goal
Verify the deployed website is publicly accessible.



Steps
Get the server's public IP.

Open the website in your browser.

Verify the footer contains your deployment details.



Commands
curl ifconfig.me

Open:

http://<YOUR_PUBLIC_IP>



Expected Output
Public IP displayed.

Website accessible through the browser.

Footer displays your Full Name and deployment information.



Screenshots Required
Screenshot 5 – Output of curl ifconfig.me.

Screenshot 6 – Browser showing the live website with your footer details.



Task 5 — Mini Real DevOps Operational Check
Goal
Verify the deployed website and Nginx service are healthy.



Steps
Verify Nginx starts automatically.

Test the Nginx configuration.

Verify the website responds locally.



Commands
systemctl is-enabled nginx

sudo nginx -t

curl -I http://localhost



Expected Output
Nginx is enabled.

Configuration test successful.

HTTP response returns 200 OK.



Screenshots Required
Screenshot 7 – Output of systemctl is-enabled nginx

Screenshot 8 – Output of curl -I http://localhost



8. Industry Insight
In production environments, static websites are commonly hosted using Nginx because it is lightweight, reliable, and capable of serving static content efficiently. DevOps engineers typically automate this deployment process using CI/CD pipelines.



9. Submission Instructions
Complete all tasks in sequence.

Your submission must include:

Screenshot 0 – Nginx service status

Screenshot 1 – Website files downloaded

Screenshot 2 – Footer updated

Screenshot 3 – Nginx configuration test

Screenshot 4 – Website files deployed

Screenshot 5 – Public IP

Screenshot 6 – Live website

Screenshot 7 – Nginx enabled

Screenshot 8 – Local HTTP response

Submit only a Google Doc link.

Follow the Assignment Submission Guidelines.



10. Solution Walkthrough
A step-by-step solution and troubleshooting guide is available for reference.

Full solution walkthrough → Click here

Step by step solution → Click here

References → Click here

Error handling & troubleshooting → Click here



11. LinkedIn Requirement (Mandatory)
Create a LinkedIn post including:

Your website URL (public IP).

3–5 lines describing what you deployed.

Screenshot of the live website showing your Full Name in the footer.

Submit:

LinkedIn Post URL

Screenshot of the LinkedIn post



12. Completion Checklist
Before submission, verify:

Website is publicly accessible.

Footer contains your Full Name.

Group number is included.

Week number is included.

Date is included.

Nginx configuration test is successful.

Nginx service is running.

Website remains online for at least 24 hours.

All required screenshots are included.

Google Doc is accessible.

Link is tested in incognito mode.



13. Final Submission
Submit only your Google Doc link.

Questions for this assignment
1. Based on the instructions and tasks above, submit your completed document with all required explanations and screenshots.

