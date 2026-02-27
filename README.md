# EM-CoPilot
EM Co Pilot Project on N8N

## Engineering Manager Team Health Copilot

**What it does**

A daily Slack briefing for engineering managers that surfaces risks from GitHub pull requests.

- Fetches open PRs from GitHub
- Computes basic health metrics:
  - Total PRs
  - Open PRs
  - Stale PRs (older than _N_ days, configurable)
- Flags risks (e.g. “Stale PRs – 2 PRs older than 4 days (owners: quantum-geek)”)
- Sends a formatted summary to a Slack channel (`#em-copilot`) every day

**Tech stack**

- [n8n](https://n8n.io/) – workflow engine
- GitHub API – source of PR data
- Slack bot – destination for the daily briefing
- GPT-4 (optional) – can be used to generate richer natural-language summaries

**Workflow file**

- `workflows/em-team-health-copilot.json`

**How it works (high level)**

1. **Daily EM Briefing Trigger**  
   Scheduled trigger that runs once per day.

2. **EM Copilot Configuration**  
   Static config (team name, Slack channel, GitHub URLs, thresholds like `stalePrDays`).

3. **Fetch GitHub PR Data**  
   n8n HTTP/GitHub node that calls the repo’s `/pulls` API and returns all open PRs.

4. **Aggregate Metrics & Flag Risks (Code node)**  
   - Counts `totalPRs`, `openPRs`  
   - Calculates `stalePrDays` from configuration  
   - Builds a `risks[]` array such as:  
     `[{ type: "Stale PRs", detail: "2 PR(s) older than 4 day(s)", owners: ["quantum-geek"] }]`

5. **Format Daily Briefing (Code node)**  
   Transforms stats and risks into a Slack-friendly message body.

6. **Send Briefing to Slack**  
   Posts the formatted message into `#em-copilot` using a Slack bot token.

**Example Slack output**

> Daily Team Health – Team Alpha  
> There are 1 active risk(s) on your board.  
>  
> **Top Risks:**  
> • Stale PRs – 2 PR(s) older than 4 day(s) (owners: quantum-geek)  
>  
> **Recommended Actions:** None  
> _Automated with this n8n workflow_
