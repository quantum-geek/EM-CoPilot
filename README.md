# Engineering Manager Copilot - Daily Team Health Briefing

Engineering Manager Copilot is an n8n based workflow that turns GitHub pull request activity and Jira work item data into a daily Slack briefing for engineering leaders.

The workflow is designed to answer a simple question every morning:

**What needs my attention on the team today?**

It does this by collecting delivery signals, identifying stale work, generating AI assisted risk commentary, and posting a concise Slack summary with recommended actions.

## What it does

- Pulls Jira issue and bug data for the team
- Pulls GitHub pull request data from the target repository
- Detects stale PRs based on configurable thresholds
- Aggregates board level risk metrics such as:
  - unassigned issues
  - open bugs
  - stale PRs
- Uses AI to summarize team health and recommend actions
- Sends a polished daily Slack briefing to a configured channel

## Why this project matters

Engineering leaders rarely have one place that combines execution signals, code review backlog, and team level operational risk into a lightweight daily summary.

This workflow closes that gap by creating a repeatable manager copilot experience that is:

- automated
- configurable
- explainable
- easy to extend

## Workflow overview

The workflow has four major stages:

1. **Data collection**  
   Pull Jira and GitHub data and normalize it into a clean intermediate shape.

2. **Signal detection**  
   Detect stale PRs and build a structured team snapshot.

3. **AI analysis**  
   Generate PR specific and team health specific commentary.

4. **Delivery**  
   Format a Slack friendly daily briefing and send it to the target channel.

## Current output

The latest version of the workflow produces a Slack briefing with:

- PR Aging Alerts
- AI Risk Insights
- Health Status
- Top Risks
- Recommended Actions

## Example use cases

- Daily engineering manager standup prep
- Staff meeting prep for delivery health review
- Early warning for review bottlenecks and ownership gaps
- Lightweight operational dashboard via Slack instead of a full custom app

## Architecture notes

### Inputs
- Jira issues and bugs
- GitHub pull requests
- n8n configuration values such as stale PR thresholds and Slack channel

### Processing
- Normalize Jira payloads
- Select GitHub PR fields
- Merge source data into a team snapshot
- Detect stale PRs
- Aggregate metrics and flag risks
- Run AI powered analysis for PR risk and team health

### Outputs
- Daily Slack message
- Action oriented summary for engineering leadership

## Configuration

The workflow currently supports configurable values such as:

- `teamName`
- `slackChannel`
- `stalePrDays`
- `staleIssueDays`

## Future enhancements

- Slash command support via `/emcopilot`
- Trend tracking over multiple days
- Repository level filtering for multiple teams
- More structured action recommendation categories
- Historical storage in a database or Google Sheet
- Rich Slack blocks for cleaner formatting

## Tech stack

- n8n
- GitHub integration
- Jira integration
- OpenAI chat model nodes
- Slack integration
- Javascript

## Repo structure

```text
.
├── README.md
├── docs/
│   ├── workflow-node-guide.pdf
│   └── demo-script.md
├── screenshots/
└── workflow/
```

## How to run

1. Import the workflow into n8n.
2. Configure GitHub, Jira, Slack, and OpenAI credentials.
3. Set team configuration values.
4. Test each node from left to right.
5. Enable the trigger once the end to end Slack output looks correct.

## What makes this strong

This project demonstrates:

- workflow orchestration
- AI assisted operational analysis
- signal extraction from engineering systems
- executive friendly communication in Slack
- practical product thinking instead of a toy demo

