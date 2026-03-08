# Engineering Manager Copilot

## Overview
Engineering Manager Copilot is an n8n-based Slack command and daily briefing system that helps engineering managers quickly understand team execution health across GitHub and Jira.

It combines:
- a scheduled daily team health briefing
- an interactive Slack `/emcopilot` command workflow
- deterministic metrics and filtering
- AI-assisted delivery risk analysis for richer risk narratives

The system reduces the need to manually jump between GitHub, Jira, and Slack to understand delivery risk, ownership gaps, PR backlog, and team execution status.

## What it does
The project has two complementary flows.

### 1. Daily Team Health Briefing
A scheduled n8n workflow runs automatically and:
- fetches GitHub pull request data
- fetches Jira issue data
- detects stale PRs
- aggregates delivery signals
- generates AI-assisted team health commentary
- posts a concise daily summary to Slack

### 2. Interactive Slack `/emcopilot` Workflow
A Slack slash-command workflow lets a manager request operational views on demand.

Supported commands:
- `/emcopilot prs`
- `/emcopilot bugs`
- `/emcopilot issues`
- `/emcopilot unassigned`
- `/emcopilot summary`
- `/emcopilot risks`
- `/emcopilot help`

Unknown or garbled commands safely fall back to the help menu.

## Why this project matters
Engineering leaders often lose time stitching together execution signals from multiple systems. This project turns those raw signals into action-oriented views.

## Architecture
### A. Slack `/emcopilot` workflow
Slack Webhook  
→ Immediate Ack to Slack  
→ Parse Command  
→ Route Command  
→ command-specific branches  
→ Merge Messages  
→ Send to Slack `response_url`

Command branches:
- PR branch: Fetch GitHub PRs → Reduce PR Data → Format PR Message
- Issue branch: Fetch Jira Issues → Reduce Issue Data → Format Issue Message
- Unassigned branch: Fetch Unassigned Jira Issues → Reduce Unassigned Data → Format Unassigned Message
- Summary branch: Fetch GitHub PRs Summary + Fetch Jira Issues Summary → reduce both → merge → aggregate → format summary
- Risks branch: Fetch GitHub PRs Risks + Fetch Jira Issues Risks → reduce both → merge → aggregate structured risk data → AI Risk Summarizer → format message
- Help branch: Build Help Message

### B. Daily Team Health workflow
Daily Trigger  
→ Load configuration  
→ Fetch GitHub PR data  
→ Fetch Jira issue data  
→ Normalize and merge  
→ Detect PR aging risks  
→ Aggregate metrics  
→ AI risk / team health analysis  
→ Format daily briefing  
→ Send Slack message

## Technical design choices
- deterministic routing and filtering first
- AI added where interpretation adds the most value
- safe fallback to help for unknown inputs
- explicit merge points for parallel branches
- preserve response metadata through each branch

## Example command behavior
- `/emcopilot prs` → PR view
- `/emcopilot bugs` → Jira issue view
- `/emcopilot issues` → Jira issue view
- `/emcopilot unassigned` → only unassigned issues
- `/emcopilot summary` → compact team summary
- `/emcopilot risks` → AI-assisted risk summary with recommended actions
- `/emcopilot ???` → help menu

## Future improvements
- trend analysis over time
- richer risk scoring
- team-specific filters
- persistence and historical reporting

**Webhook Flow**
<img width="1348" height="692" alt="image" src="https://github.com/user-attachments/assets/b43233d9-9938-4f29-a014-d0fd51c49e7c" />

**/emcopilot risks**

<img width="891" height="307" alt="image" src="https://github.com/user-attachments/assets/b5072e71-e634-4a09-939d-d054b0879856" />

**/emcopilot issues**

<img width="853" height="411" alt="image" src="https://github.com/user-attachments/assets/6907a47c-9a7b-46f4-ba44-797361de5f69" />

**/emcoppilot prs**

<img width="702" height="257" alt="image" src="https://github.com/user-attachments/assets/b2623067-d677-473a-a168-7aec75ef6c47" />

**/emcopilot help**

<img width="946" height="448" alt="image" src="https://github.com/user-attachments/assets/5062b7e3-4e7a-456f-a725-bed4ee175d90" />


**Daily EM Briefing Trigger**

<img width="1877" height="427" alt="image" src="https://github.com/user-attachments/assets/c365443a-d85e-48c8-b648-994addea8882" />

**Daily EM Briefing Trigger Output**

<img width="868" height="523" alt="image" src="https://github.com/user-attachments/assets/8ef9c9a1-fbfc-4376-80c3-5bd8eeaf971b" />

