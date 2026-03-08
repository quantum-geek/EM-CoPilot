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

Webhook Flow
<img width="1348" height="692" alt="image" src="https://github.com/user-attachments/assets/b43233d9-9938-4f29-a014-d0fd51c49e7c" />
