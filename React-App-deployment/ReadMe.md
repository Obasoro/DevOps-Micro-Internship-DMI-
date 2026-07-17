Assignment: Deploy a React App on Ubuntu VM Using Nginx


1. Assignment Overview
Assignment: Deploy a React App on Ubuntu VM Using Nginx
Estimated Time: 60 minutes
Difficulty: Intermediate
Category: AWS / Linux



2. Objective
In this assignment, you will deploy a React application on an Ubuntu VM and serve it using Nginx.

This assignment helps you understand real-world Linux server setup, React build deployment, and web server configuration.



3. Real-World Scenario
EpicReads, an online bookstore, is migrating to the cloud. As a DevOps Engineer, you are responsible for deploying web applications to cloud servers.

Your manager expects you to deploy a React application on an Ubuntu VM and provide proof that it is accessible via a public IP with your personalized details.



4. Learning Outcomes
By the end of this assignment, you will be able to:

Provision and configure an Ubuntu EC2 instance for hosting web applications

Install and manage Node.js, npm, and Nginx on a Linux environment

Deploy and serve a production-ready Single Page Application (SPA) using Nginx



5. Important Instructions (Global Rules)
Follow the Assignment Submission Guidelines Click here

Key Rules:
Full Name must be visible in required screenshots

Do not expose sensitive information (keys, passwords, account IDs)

Follow screenshot requirements exactly as specified in tasks

Submission must clearly match task outputs

Missing or incorrect proof may result in rejection



6. Prerequisites
Required:
Ubuntu EC2 instance (running)

Basic Linux command knowledge

Internet access on VM

AWS Free Tier account

EC2 Prerequisites:
Ensure your EC2 instance has:

Ubuntu AMI selected

Security Group configured:

Inbound rules:

SSH (22) from your IP (or anywhere for testing)

HTTP (80) from anywhere

Auto-assign Public IP enabled

Instance in Running state



7. Tasks
Each task must be completed sequentially.



Task 1 — Setup Environment (Node.js & npm)
Goal
Install required software for deployment.

Steps
Update system packages

Install Node.js and npm

Verify installation

Commands
sudo apt update && sudo apt upgrade -y

sudo apt install -y nodejs npm

node -v && npm -v

Expected Output
Node.js, npm versions - From command 3

Screenshots Required
Screenshot 1 - Output of command 3 (node -v && npm -v)



Task 2 — Setup Environment (nginx)
Goal
Install required software for deployment.

Steps
Install nginx

Verify installation

Start nginx

Enable nginx

Check nginx status

Commands
sudo apt install -y nginx

nginx -v

sudo systemctl start nginx

sudo systemctl enable nginx

systemctl status nginx --no-pager

Expected Output
Nginx version - From command 2

Active(running) - From command 5

Screenshots Required
Screenshot 2 - Output of command 5 (systemctl status nginx --no-pager)



Task 3 — Clone React Application
Goal
Download the project repository.

Steps
Clone repository

Navigate to the project folder

List the folder contents

Commands
git clone https://github.com/pravinmishraaws/my-react-app.git

cd my-react-app

ls

Expected Output
React project files visible in my-react-app directory.

Screenshots Required
Screenshot 3 - Output of command 3 (ls)



Task 4 — Modify Application (Personalization)
Goal
Update React app with your details.

Steps
Navigate to src folder

Open App.js (nano/vi editor)

Replace placeholder values with your Full Name and Date

Save your work

Exit from file

Commands
cd src

nano App.js

Modify the content: Replace placeholder values with your details

<h2>Deployed by: <strong>Your Full Name</strong></h2>
<p>Date: <strong>DD/MM/YYYY</strong></p>
Press CTRL + O

Press Enter

Press CTRL + X

Expected Output
App.js updated with personal details.

Screenshots Required
Screenshot 4 - Output of “nano App.js” (Take this after editing the file - Edited App.js showing name and date)



Task 5 — Build React Application
Goal
Create production build.

Steps
Navigate back to the project root

Install dependencies

Build the application

List the contents

Commands
cd ..

npm install

npm run build

ls

Expected Output
build/ folder generated successfully.

Screenshots Required
Screenshot 5 - output of command 4 (ls)



Task 6 — Deploy React Build to Nginx Web Root
Goal
Deploy React build to web server.

Steps
Clear existing Nginx default web files

Copy build files to /var/www/html

Set correct ownership

Set proper file permissions

List the contents of the folder

Commands
sudo rm -rf /var/www/html/*

sudo cp -r build/* /var/www/html/

sudo chown -R www-data:www-data /var/www/html

sudo chmod -R 755 /var/www/html

ls /var/www/html/

Expected Output
build/ folder contents copied to the nginx web root folder

Screenshots Required
Screenshot 6 - output of command 5 (ls /var/www/html/)



Task 7 — Configure Nginx for React Application
Goal
Configure Nginx for React routing.

Steps
Replace default Nginx configuration

Test nginx configuration

Restart nginx service

Verify nginx is running

Commands
echo 'server {
listen 80;
server_name _;
root /var/www/html;
index index.html;
 
location / {
try_files $uri /index.html;
}
 
error_page 404 /index.html;
}' | sudo tee /etc/nginx/sites-available/default > /dev/null


sudo nginx -t

sudo systemctl restart nginx

systemctl is-active nginx

cat /etc/nginx/sites-available/default



Expected Output
N/A

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
N/A

active

Nginx running with valid configuration.

Screenshots Required
Screenshot 7 - output of command 4 (systemctl is-active nginx)

Screenshot 8 - output of command 5 (cat /etc/nginx/sites-available/default)



Task 8 — Test Deployment
Goal
Verify application is accessible.

Steps
Get the public IP address of the server

Access the application in browser:

Commands
curl ifconfig.me

Open a browser and visit: http://<your-public-ip>

Replace <your-public-ip> with the IP obtained from the previous command.

Expected Output
server’s public IP address.

React app accessible via: http://<public-ip>

→ Your name and date updated in the application

Screenshots Required
Screenshot 9 - output of command 1 (curl ifconfig.me)

Screenshot 10 - browser output



8. Industry Insight (Optional)
In real DevOps workflows, React applications are built locally and deployed to production servers using automated CI/CD pipelines. Nginx is commonly used as a reverse proxy and static file server.



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



11. LinkedIn Requirement (If Applicable)
Create a LinkedIn post including:

What you deployed

3–5 lines explanation

Screenshot of working application

Submit:

LinkedIn post URL

Screenshot of post



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

