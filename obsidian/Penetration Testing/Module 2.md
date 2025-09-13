# Penetration Test Process

General Timeline:
Pre-Test -> Test -> Post-Test 
0-4               4-5        5-7

## Pre-test
Intake - new request for a test, with optional questionnaire or other forms
Kick off meeting - customer interview, define testing scope, define rules of engagement
Authorization and protection docs
Legal docs
Planning
	Prepare toolkit
	Develop test plan and schedule
	Assign roles

## Execute the Test
Follow test plans based on test type and methodology, take good notes, screenshots, populate reporting template
Regular communication
Special notifications - beginning and end of destructive or noisy activities, incidents and major concerns (exposed systems or data, IOCs)

## Post Test Activities
Score Risk for each finding - measure impact, calculate business value
Write and deliver report of findings
Schedule re-test to verify mitigations

# Penetration Test Preparation
## Interview
1. What are customer concerns
	1. Gov requirements - PCI, HIPPA, National Defense, etc
	2. Ransomware
	3. Downtime and lost evenue
	4. Disclouse of sensitive info
	5. Embarassment or rep
	6. Competition
2. Which assets do they want to potect
3. What are possible threats?
4. What do they want to get out of the test?

Based on customer concerns, define how the test goes
Along with how the test goes, referneced sections in Rules of Engagement

## Scope
We need crystal clear customer expectations and knowledge of scope
	Target IPs
	Subnets
	Domains
	Applications
What is to be excluded, avoided

Scpe Creep -uncontrolled expansion of the project's scope beyond original tasks

Third parties - customer may ask you test a target they don't own
	Vendor Equipment
	cloud resources
Make sure to understand targets and get written authorization

When testing cloud services, be aware of 
	who wons the cloud
	Is it shared by other tenants
	Would we need explicit pemission
When testing cloud, we often test application only

Production vs test environmnet
Ideally we want to target test environment to be less disruptive

Realistically, we should test production env because there may be differences

## ROE and Other
ROE provides both parties with the playbook for this test
1. Testing Times
2. Points of contact
3. Testing schedule, briefings, reportings
4. Types of tests authroized or not authorized
	1. Inclusive, not exclusive
5. Problem
	1. Local admins blocking pentest traffic
	2. Connectivity issues
6. Dealing with sensitive data

Any work for a customer should include a statement of work
This is where we can talk about
1. Price
2. Daily briefing requirements
3. Report deliverables and timeline
4. List all applicable documents

# Testing Toolkit
Toolkit should have variety, match target env
Software is specific to test

## Kali
Offers tools that covers most of the time

## Virtualization, Network Considerations, Securing
Virtualbox, VMware

Ideally, we want to be close - direct LAN connection
No firewall or IDS/IPS unless explicitly part of the test

For internet based scanning, working with ISP and contact them


# Testing Legally and with Authorization
Authorization is the difference between an ethical hacker and an unethical hacker

Need written authorization, liability waiver, non-disclousre agreement, backup


# Reporting and Testing Checklist
Every pentest requires a report - the proof that anything was done

Reference our report after years to understand what was done

Start taking notes and writing certain sections
## Format
1.Executive Summary
	Most important part, write it last
	Summarize project, testing methodology, overall risk, and significant findings
	Project summary - 1/2 graffs, dates goals parties overview
	testing methodology
	overall score - pass fail, low-medium high for risk
	findings - recommendations
2.Introduction
	Overview of test, include dates or a timeline, all parties involved
	scope
3.Methodology
	Descrie the process for testing and the results of each process
		Intel, recon, scanning, vuln and weakness enumeration
	Scope, inventory of all targets
	Tools
	Any steps that lead to major findings
4.Findings
5.Conclusion
6.Appendixes

