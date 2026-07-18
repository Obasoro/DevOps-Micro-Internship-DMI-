. Assignment Overview
Assignment: AI-Assisted Linux Health Check
Estimated Time: 120-150 minutes
Difficulty: Intermediate
Category: Linux / Bash / Agentic AI / DevOps Operations



2. Objective
In this assignment, you will build a read-only Bash script that checks the health of your Ubuntu server and Nginx application.

You will then connect that script to Claude Code as a reusable /linux-triage skill.

Finally, you will stop Nginx on your lab VM, use the skill to gather and analyze evidence, recover the service manually, and run the skill again to verify that the application is healthy.

This assignment helps you understand an important DevOps pattern:

Bash collects facts. Agentic AI analyzes the facts. The engineer approves the action. The system is checked again.



3. Real-World Scenario
The EpicReads React application is running on an Ubuntu EC2 instance using Nginx.

A user reports:

"The website is not opening."

As the on-call DevOps Engineer, you should not immediately restart services or make random changes. First, you need evidence:

Is Nginx running?

Is port 80 listening?

Is the application responding locally?

Is the disk nearly full?

Is enough memory available?



Typing these checks manually during every incident is slow and inconsistent. You will therefore create a reusable Bash triage script.

Claude Code will run the script through a controlled skill, read the report, explain the evidence, and recommend a safe next step. Claude must not restart the service automatically. You remain responsible for the recovery decision.



4. Learning Outcomes
By the end of this assignment, you will be able to:

Use Linux commands to check services, ports, HTTP responses, disk usage, memory, and service logs

Use Bash variables, arrays, loops, conditionals, functions, command substitution, output redirection, and exit codes

Build a repeatable Linux incident-triage script

Add project-specific operating rules to CLAUDE.md

Create and manually invoke a Claude Code skill

Restrict an AI skill to read-only tools

Use the Agentic Loop during an incident: Gather -> Analyze -> Human Act -> Verify

Separate evidence, AI recommendations, and human-approved actions

Simulate and recover from a controlled Nginx incident



5. Important Instructions (Global Rules)
Follow the Assignment Submission Guidelines: Click here



Key Rules:

Complete this assignment only on your personal lab VM or training EC2 instance

Do not stop Nginx on a production, company, shared, or customer server

Your Full Name must be visible in the Bash report and required screenshots

Do not expose passwords, private keys, tokens, AWS account IDs, or cloud credentials

The /linux-triage skill must remain read-only

Claude must not stop, start, restart, install, delete, or modify anything

Run recovery commands yourself only after reviewing the evidence

Do not use destructive commands such as rm -rf, chmod 777, reboot, or shutdown

Capture the failed-state evidence before restoring Nginx

Restore Nginx and verify the application before completing the assignment

Write all explanations in your own words

Screenshots must show both the command and its output

Missing or incorrect proof may result in rejection



6. Prerequisites
Required:

Week 2 Agentic AI assignments completed

Claude Code installed and authenticated

Basic understanding of CLAUDE.md, skills, the Agentic Loop, and tool restrictions

Ubuntu EC2 instance or Ubuntu lab VM

React application already deployed using Nginx

Nginx currently running

Claude Code available on the same Ubuntu VM

Basic Linux and Bash knowledge



Required commands:

bash

systemctl

ss

curl

df

free

awk

grep

journalctl

Before starting, confirm that the application opens successfully in the browser.



7. Tasks
Each task must be completed sequentially.



Task 1 - Confirm the Healthy Baseline and Create the Workspace
Goal
Confirm that Nginx and the React application are healthy before building the automation.



Steps
Connect to your Ubuntu VM

Confirm that Nginx is active

Confirm that port 80 is listening

Confirm that the application responds from the server

Create the assignment workspace and required folders

Move into the workspace

Display the folder structure



Commands
systemctl is-active nginx

ss -ltn | grep ':80'

curl -I http://localhost

mkdir -p ~/week-03-agentic-linux/{scripts,reports,.claude/skills/linux-triage}

cd ~/week-03-agentic-linux

pwd

find . -maxdepth 4 -type d | sort



Expected Outputs
systemctl is-active nginx returns active

Port 80 appears in the ss output

curl -I http://localhost returns an HTTP response such as 200 OK

The workspace contains:



week-03-agentic-linux/

├── .claude/

│ └── skills/

│ └── linux-triage/

├── reports/

└── scripts/



Screenshots Required
Screenshot 1 - Output of systemctl is-active nginx, ss -ltn | grep ':80', and curl -I http://localhost

Screenshot 2 - Output of pwd and find . -maxdepth 4 -type d | sort



Notes You Must Write (Very Important)
What proves that Nginx is running?

What proves that the server is listening for HTTP traffic?

Why must you capture a healthy baseline before simulating an incident?



Task 2 - Create Project Context and Safety Rules in CLAUDE.md
Goal
Tell Claude exactly what this project does and what it is not allowed to do.



Steps
Create CLAUDE.md in the assignment root

Add the project overview

Add the incident workflow

Add the operational safety rules

Save the file



Commands
cd ~/week-03-agentic-linux

nano CLAUDE.md



Add the following content:

# Project Overview
This project builds a read-only Linux and Nginx incident-triage workflow for an Ubuntu lab server.
The Bash script is responsible for collecting system evidence. Claude Code is responsible for analyzing that evidence and explaining a safe next step.

# Incident Workflow
Always follow this order:
1. Gather evidence
2. Analyze the evidence
3. Ask the human to approve and execute any recovery action
4. Verify the system again

# Safety Rules
- Never stop, start, or restart a service automatically.
- Never install or remove packages.
- Never edit Nginx configuration.
- Never delete files or directories.
- Never use rm -rf, chmod 777, reboot, or shutdown.
- Use only the Bash report as the primary source of incident evidence.
- Recommend a recovery command, but do not execute it.
- Do not claim a root cause unless the report contains supporting evidence.

# Output Rules
When analyzing a report, show:
1. Overall status
2. Failed or warning checks
3. Exact evidence from the report
4. Most likely cause
5. One safe recovery command for the human to review
6. One verification command


Expected Output
A project-level CLAUDE.md exists and clearly defines the read-only incident workflow.



Screenshots Required

Screenshot 3 - CLAUDE.md open in VS Code showing all four sections



Notes You Must Write (Very Important)
Why should Claude receive project-specific operational rules?

Why is the human required to execute the recovery command?

Which rule prevents Claude from making an unsupported diagnosis?



Task 3 - Use Agentic AI to Plan Before Writing the Script
Goal
Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.



Steps
Start Claude Code from the assignment directory

Enter the prompt below exactly

Watch which read-only tools Claude uses

Review the proposed checks

Do not allow Claude to create or edit files during this task



Commands
cd ~/week-03-agentic-linux

claude



In Claude Code, enter

Read CLAUDE.md. Do not create or edit any files. Inspect this Ubuntu server using only read-only commands. Propose a Bash incident-triage plan with exactly five checks: Nginx service status, port 80 listening state, localhost HTTP response, root disk usage, and available memory. For each check, show the Linux command, what a healthy result looks like, and what a failed result means.


Expected Output
Claude reads the project rules, performs or proposes read-only checks, and returns a five-check plan without modifying files.



Screenshots Required

Screenshot 4 - Claude Code showing the five-check plan and read-only inspection



Notes You Must Write (Very Important)
Which part of this task represents the Gather phase?

Did Claude follow the instruction not to create files? How did you verify this?

Why is planning before coding useful in DevOps automation?



Task 4 - Build the Linux Triage Bash Script
Goal
Create one Bash script that gathers consistent Linux and Nginx health evidence.



Steps
Create scripts/linux-triage.sh

Add the script below

Replace <Your Full Name> with your actual full name

Save the file

Make it executable

Validate the Bash syntax



Commands
cd ~/week-03-agentic-linux

nano scripts/linux-triage.sh



Add the following content:

#!/bin/bash
 
set -u
full_name="<Your Full Name>"
service_name="nginx"
target_url="http://localhost"
disk_warning_threshold=80
disk_failure_threshold=90
memory_warning_mb=100
base_dir="$(cd "$(dirname "${BASH_SOURCE[0]}")/.." && pwd)"
report_dir="$base_dir/reports"
report_file="$report_dir/linux-health-report.txt"
 
checks=(
check_service
check_port
check_http
check_disk
check_memory
)
 
pass_count=0
warning_count=0
failure_count=0
 
mkdir -p "$report_dir"
 
: > "$report_file"
 
write_line() {
echo "$1" | tee -a "$report_file"
}
 
mark_pass() {
write_line "[PASS] $1"
pass_count=$((pass_count + 1))
}
 
mark_warning() {
write_line "[WARN] $1"
warning_count=$((warning_count + 1))
}
 
mark_failure() {
write_line "[FAIL] $1"
failure_count=$((failure_count + 1))
}
 
print_header() {
write_line "========================================"
write_line "Linux Incident Triage Report"
write_line "========================================"
write_line "Full Name: $full_name"
write_line "Timestamp: $(date -u '+%Y-%m-%dT%H:%M:%SZ')"
write_line "Hostname: $(hostname)"
write_line "Target Service: $service_name"
write_line "Target URL: $target_url"
write_line ""
}
 
check_service() {
if systemctl is-active --quiet "$service_name"
then
mark_pass "Nginx service is active"
else
mark_failure "Nginx service is not active"
fi
}
 
check_port() {
if ss -ltn | awk 'NR > 1 {print $4}' | grep -qE ':80$'
then
mark_pass "Port 80 is listening"
else
mark_failure "Port 80 is not listening"
fi
}
 
check_http() {
local http_code
http_code=$(curl -s -o /dev/null -w '%{http_code}' --max-time 5 "$target_url" || true)
if [ -z "$http_code" ]
then
http_code="000"
fi
 
if [ "$http_code" = "200" ]
then
mark_pass "Local HTTP check returned status $http_code"
else
mark_failure "Local HTTP check returned status $http_code"
fi
}
 
check_disk() {
local disk_usage
disk_usage=$(df -P / | awk 'NR == 2 {gsub("%", "", $5); print $5}')
if [ "$disk_usage" -ge "$disk_failure_threshold" ]
then
mark_failure "Root disk usage is ${disk_usage}%"
elif [ "$disk_usage" -ge "$disk_warning_threshold" ]
then
mark_warning "Root disk usage is ${disk_usage}%"
else
mark_pass "Root disk usage is ${disk_usage}%"
fi
}
 
check_memory() {
local available_memory
available_memory=$(free -m | awk '/^Mem:/ {print $7}')
if [ "$available_memory" -lt "$memory_warning_mb" ]
then
mark_warning "Available memory is ${available_memory} MB"
else
mark_pass "Available memory is ${available_memory} MB"
fi
}
 
capture_recent_logs() {
local log_output
write_line ""
write_line "Recent Nginx service logs (last 5 lines):"
log_output=$(journalctl -u "$service_name" --no-pager -n 5 2>/dev/null || true)
if [ -n "$log_output" ]
then
echo "$log_output" | sed 's/^/ /' | tee -a "$report_file"
else
write_line " No readable Nginx journal entries were found."
fi
}
 
print_summary() {
local overall_status
local script_exit_code
if [ "$failure_count" -gt 0 ]
then
overall_status="FAIL"
script_exit_code=2
elif [ "$warning_count" -gt 0 ]
then
overall_status="WARN"
script_exit_code=1
else
overall_status="HEALTHY"
script_exit_code=0
fi
 
write_line ""
write_line "Summary:"
write_line "PASS: $pass_count"
write_line "WARN: $warning_count"
write_line "FAIL: $failure_count"
write_line "Overall Status: $overall_status"
write_line "Script Exit Code: $script_exit_code"
write_line "Report File: $report_file"
return "$script_exit_code"
}
 
print_header
 
for check_function in "${checks[@]}"
do
"$check_function"
done
 
capture_recent_logs
print_summary
exit $?


Run:

chmod +x scripts/linux-triage.sh

bash -n scripts/linux-triage.sh

ls -l scripts/linux-triage.sh



Expected Outputs
bash -n scripts/linux-triage.sh returns no syntax error

The script has executable permission

The script contains variables, an array, a loop, conditionals, functions, command substitution, report redirection, and exit codes



Screenshots Required
Screenshot 5 - Top section of linux-triage.sh showing variables, thresholds, and the checks array

Screenshot 6 - Middle section showing check functions and conditionals

Screenshot 7 - Bottom section showing the loop, summary function, and exit behavior

Screenshot 8 - Output of bash -n scripts/linux-triage.sh and ls -l scripts/linux-triage.sh



Notes You Must Write (Very Important)
What is stored in the checks array?

How does the for loop use that array?

Why are the health checks separated into functions?

What is the purpose of $(...) in this script?

Why does the script use different exit codes for HEALTHY, WARN, and FAIL?



Task 5 - Run and Understand the Healthy-State Report
Goal
Run the Bash script against the healthy server and verify that it creates a report.



Steps
Run the script

Capture its exit code immediately

Display the report

Confirm that there are no failed checks before continuing



Commands
cd ~/week-03-agentic-linux

./scripts/linux-triage.sh

script_exit_code=$?

echo "Captured Exit Code: $script_exit_code"

cat reports/linux-health-report.txt



Expected Outputs
Your Full Name appears in the report

Nginx service check passes

Port 80 check passes

Local HTTP check returns 200

Disk and memory checks show PASS or WARN

The baseline report contains no FAIL result

Overall Status is HEALTHY or WARN

The captured exit code matches the report



Important: Do not continue to the incident simulation if the baseline report contains FAIL. Fix the existing lab setup first.



Screenshots Required
Screenshot 9 - Output of ./scripts/linux-triage.sh showing your Full Name and all five checks

Screenshot 10 - Output showing the captured exit code and final summary



Notes You Must Write (Very Important)
What is the overall status of your healthy baseline?

Which exact Linux evidence proves the application is serving traffic?

Did your script return exit code 0 or 1? Explain why.

What is the difference between a warning and a failure in this script?



Task 6 - Create and Run the /linux-triage Skill
Goal
Turn the Bash script into a reusable, manually invoked Agentic AI workflow.



Steps
Create .claude/skills/linux-triage/SKILL.md

Add the skill definition below

Save the file

Close and reopen Claude Code so the skill is loaded

Run /linux-triage

Confirm that Claude reads the report and does not modify the server



Commands
cd ~/week-03-agentic-linux

nano .claude/skills/linux-triage/SKILL.md



Add the following content:



---
name: linux-triage
description: Run the read-only Linux and Nginx health-check script, analyze its evidence, and produce a concise incident-triage report.
allowed-tools: Bash, Read, Grep
disable-model-invocation: true
---

# Linux Triage Skill
When `/linux-triage` is invoked:
1. Read `CLAUDE.md` before doing anything else.
2. Run `bash scripts/linux-triage.sh || true`.
3. Read `reports/linux-health-report.txt`.
4. Report the following:

- Overall status
- Every WARN or FAIL result
- Exact evidence from the report
- Most likely cause supported by the evidence
- One safe recovery command for the human to review
- One verification command

5. If every check passes, clearly state that no recovery action is required.
6. Do not edit files.
7. Do not use sudo.
8. Do not stop, start, restart, install, delete, or modify anything.
9. Never execute the recovery command.
10. Ask the human to review and run any recovery action manually.


Start a new Claude Code session:

claude



Run:

/linux-triage



Expected Outputs
Claude invokes the Bash script

Claude reads linux-health-report.txt

Claude reports the healthy or warning state using evidence

Claude does not restart Nginx or change any file

If no check failed, Claude states that no recovery action is required



Screenshots Required
Screenshot 11 - SKILL.md showing the frontmatter, tool restrictions, and safety rules

Screenshot 12 - /linux-triage output for the healthy server



Notes You Must Write (Very Important)
Why does this skill have Bash, Read, and Grep, but not Write?

Why is disable-model-invocation: true useful for this skill?

What part is performed by Bash, and what part is performed by Claude?

Why is this better than asking Claude, "Is my server healthy?" without giving it evidence?



Task 7 - Simulate an Nginx Incident and Let the Skill Diagnose It
Goal
Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.



Warning
Perform this task only on your personal lab VM.



Steps
Exit Claude Code or open a separate regular terminal

Stop Nginx manually

Confirm that the service is inactive

Confirm that the local HTTP request fails

Run /linux-triage in Claude Code

Confirm that Claude identifies the failed checks

Confirm that Claude recommends a recovery command but does not execute it

Save the failed-state report before recovery



Commands
in the regular terminal:

sudo systemctl stop nginx

systemctl is-active nginx

curl -I --max-time 5 http://localhost



In Claude Code:

/linux-triage



After the skill finishes, run in the regular terminal:

cp reports/linux-health-report.txt reports/incident-failure-report.txt

cat reports/incident-failure-report.txt



Expected Outputs
Nginx state is inactive

The HTTP request cannot connect or does not return 200

The Bash report shows FAIL for Nginx service status

The Bash report shows FAIL for port 80

The Bash report shows FAIL for the local HTTP check

Claude uses the report as evidence

Claude recommends a safe command such as sudo systemctl start nginx

Claude does not execute the recovery command

reports/incident-failure-report.txt is created



Screenshots Required
Screenshot 13 - Output showing Nginx is inactive and the HTTP request fails

Screenshot 14 - /linux-triage output showing failed evidence, likely cause, and a suggested recovery command

Screenshot 15 - incident-failure-report.txt showing the failed checks and your Full Name



Notes You Must Write (Very Important)
Which three checks failed?

What evidence supports the conclusion that Nginx is unavailable?

Did Claude execute the recovery command? Why is that important?

Which phase of the Agentic Loop is represented by the Bash report?

Which phase is represented by Claude's explanation?



Task 8 - Recover Manually, Verify Again, and Write the Incident Summary
Goal
Recover the service as the human operator and prove that the system is healthy again.



Steps
Review the recovery recommendation

Start Nginx manually

Confirm that Nginx is active

Confirm that the application responds locally

Run /linux-triage again

Save the recovery report

Write a short incident summary in your own words

List all report files



Commands in the regular terminal
sudo systemctl start nginx

systemctl is-active nginx

curl -I http://localhost



In Claude Code:

/linux-triage



After the skill finishes, run:

cp reports/linux-health-report.txt reports/recovery-report.txt

ls -lah reports



Create incident-summary.md:

nano incident-summary.md



Use the following structure:

# Nginx Incident Summary
**Full Name:** <Your Full Name>
**Date:** <DD/MM/YYYY>

## 1. Reported Symptom
Explain what appeared to be broken.

## 2. Evidence Collected
List the failed Bash checks and the exact evidence they showed.

## 3. Most Likely Cause
Explain the cause using only the collected evidence.

## 4. Human-Approved Recovery Action
Write the command you reviewed and executed manually.

## 5. Verification
Explain which outputs proved that Nginx and the application recovered.

## 6. Safety Decision
Explain why the AI skill was allowed to gather and analyze evidence but was not allowed to restart the service.

## 7. Agentic Loop Mapping
Explain how this incident followed:

Gather -> Analyze -> Human Act -> Verify


Expected Outputs
Nginx returns to active

curl -I http://localhost returns 200 OK

The second /linux-triage run contains no FAIL result

recovery-report.txt is created

incident-summary.md contains all seven required sections

Both failed and recovered evidence remain available for comparison



Screenshots Required
Screenshot 16 - Output showing Nginx is active and the HTTP request returns 200

Screenshot 17 - Second /linux-triage output showing successful recovery

Screenshot 18 - Output of ls -lah reports showing incident-failure-report.txt and recovery-report.txt

Screenshot 19 - incident-summary.md showing all required sections and your Full Name



Notes You Must Write (Very Important)
What action did you execute manually?

What evidence proves that the service recovered?

Why is the second triage run necessary?

What could go wrong if an AI agent automatically restarted every failed service?

In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.



8. Industry Insight
In production operations, the first job of an engineer is not to guess. The first job is to gather reliable evidence.

Bash is useful for this because it runs the same checks in the same order every time. The output is deterministic and easy to save as an incident artifact.

Agentic AI adds a different capability. It can read the evidence, connect related failures, explain the likely cause, and present a safe next step. However, an AI explanation is still a recommendation. High-impact actions should remain controlled by permissions, approvals, and human judgment.

This pattern is common in modern DevOps workflows:



Automated evidence collection -> AI-assisted analysis -> human-approved action -> automated verification



9. Submission Instructions
Complete all tasks in sequence.

Your submission must include:

All 19 required screenshots

Answers to every "Notes You Must Write" question

CLAUDE.md

scripts/linux-triage.sh

.claude/skills/linux-triage/SKILL.md

reports/incident-failure-report.txt

reports/recovery-report.txt

incident-summary.md

GitHub folder or repository URL containing the assignment files

Your Full Name visible in the required outputs



Submit only a Google Doc link.

Add the GitHub URL inside the Google Doc.

Follow the Assignment Submission Guidelines (LINK).



10. Solution Walkthrough (Solution + Error Handling)
A step-by-step solution and troubleshooting guide is available for reference:

Full solution walkthrough -> Click here

Common errors to check:

Permission denied when running the script: Run chmod +x scripts/linux-triage.sh

Skill does not appear: Close Claude Code completely and start a new session from the assignment root

systemctl command not found or system not booted with systemd: Confirm you are running the assignment on the Ubuntu VM, not inside an unsupported shell environment

Port 80 does not appear during the baseline: Confirm Nginx is active before continuing

HTTP code is 000: The connection failed or timed out

Baseline contains FAIL: Repair the existing Nginx deployment before simulating the incident

Report file not created: Confirm the reports directory exists and the script path is correct

Unexpected WARN for memory or disk: Review the threshold and explain the warning; do not hide it

Claude tries to take action: Stop the tool request, review CLAUDE.md and SKILL.md, and confirm the safety rules are present

Nginx remains inactive at the end: Run sudo systemctl start nginx and verify again before submitting



11. LinkedIn Requirement
Create a LinkedIn post including:

What you built: a Bash-based Linux incident-triage script and a Claude Code skill

What incident you simulated: Nginx stopped on a lab VM

What the workflow demonstrated: evidence gathering, AI analysis, human-approved recovery, and verification

One lesson about why AI should not receive unrestricted operational access

Screenshot of the failed /linux-triage result

Screenshot of the recovered /linux-triage result

Write 4-6 lines in your own words.



Suggested tags:

#DMIByPravinMishra #Linux #Bash #AgenticAI #ClaudeCode #DevOps


Submit:

LinkedIn post URL

Screenshot of the published post



12. Completion Checklist
Before submission, verify:



Healthy baseline captured before the incident

Workspace structure created correctly

CLAUDE.md contains project context and safety rules

Claude produced a read-only five-check plan

linux-triage.sh passes bash -n

Your Full Name appears in the Bash report

Bash script uses variables, arrays, loops, conditionals, functions, and exit codes

Healthy baseline contains no FAIL result

/linux-triage skill loads and runs successfully

Skill does not have Write permission

Skill does not execute recovery actions

Nginx failure was simulated only on the lab VM

Failed-state report was saved before recovery

Nginx was restored successfully

Recovery report contains no FAIL result

incident-summary.md is complete

All required screenshots are included

All written answers are in your own words

No sensitive data is exposed

GitHub URL is included in the Google Doc

Google Doc is accessible

Link is tested in incognito mode



13. Final Submission
Submit only your Google Doc link.

Questions for this assignment
1. Based on the instructions and tasks above, submit your completed document with all required explanations, screenshots, reports, script files, skill file, incident summary, and GitHub URL.

