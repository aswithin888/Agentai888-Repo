# Agentai888-Repo
AI Workforce Infrastructure for GoHighLevel — 7 autonomous AI agents handle lead enrichment, personalized outreach, follow-up sequences, appointment booking, CRM updates, policy enforcement, and full GHL browser automation. Built by The Alleges Group Inc. Powered by Claude AI + Playwright + FastAPI.
# AgentAI888 × GoHighLevel
## AI Workforce Infrastructure

> **The Alleges Group Inc.** — A fully autonomous revenue engine that deploys 7 AI agents inside GoHighLevel to handle every step of the sales process without human intervention.

---

## What This Does

AgentAI888 connects to your GoHighLevel account via webhooks and deploys an autonomous AI workforce that runs 24/7. The moment a new lead enters GHL, your robots take over:

```
New Lead Enters GHL
       ↓
Lead Intel Agent — scores and enriches the contact
       ↓
Outreach Agent — sends personalized SMS within 60 seconds
       ↓
Follow-Up Agent — runs Day 1 / Day 3 / Day 7 sequences
       ↓
Appointment Agent — books the discovery call
       ↓
CRM Agent — updates all fields, adds notes, moves the deal
       ↓
AIONexus — logs every action in a hash-chained audit ledger
       ↓
Revenue — without a human touching anything
```

The **Browser Agent** operates GHL exactly like a human — building funnels, buying phone numbers, setting up workflows, responding to inbox messages — all from a plain-English command.

---

## The 7 AI Agents

| Agent | Role | Trigger |
|---|---|---|
| **Lead Intel** | Enriches contact, assigns AI lead score 0-100, routes to workflow | contact_created webhook |
| **Outreach** | Personalized SMS and email via GHL API | tag: ai-outreach |
| **Follow-Up** | Day 1/3/7 sequences, SLA monitoring | tag: ai-followup |
| **Appointment** | Books calls, sends calendar confirmations, moves pipeline | tag: ai-book |
| **CRM Agent** | Updates fields, adds notes, tags contacts, moves deals | tag: ai-crm |
| **AIONexus** | Policy engine + hash-chained audit ledger | All events |
| **Browser Agent** | Full GHL UI automation via Playwright + Claude computer-use | On demand / scheduled |

---

## Architecture

```
[GoHighLevel CRM]
       | Webhooks
       v
[Agent Router] — parses payload, checks duplicate lock, dispatches
       |
       +---> [Lead Intel] → [Outreach] → [Follow-Up] → [Appointment] → [CRM Agent]
       |                        GHL API write-back on every step
       |
       +---> [AIONexus] — policy checks + audit logging on every action
       |
       +---> [Browser Agent] — Playwright + Claude computer-use → operates GHL UI
                    |
                    +---> HITL Gate — Slack notifications for destructive actions
```

---

## File Structure

```
agentai888/
├── server/
│   └── main.py                    # FastAPI server — all endpoints
├── router/
│   └── agent_router.py            # Webhook parser + duplicate lock + dispatcher
├── agents/
│   ├── lead_intel.py              # Claude-powered scoring + GHL enrichment
│   ├── outreach.py                # Personalized SMS/email via GHL API
│   ├── followup.py                # Multi-touch sequences + SLA monitoring
│   ├── appointment.py             # Booking + pipeline stage movement
│   ├── crm_agent.py               # Field updates + notes + tagging
│   └── aionexus.py                # Policy engine + hash-chained audit log
├── browser_agent/
│   ├── browser_agent.py           # Core Playwright + Claude computer-use loop
│   ├── ghl_login.py               # GHL login handler + 2FA via TOTP
│   ├── hitl.py                    # Human-in-the-loop Slack notifications
│   └── funnel_intelligence.py     # Industry detection + funnel blueprint engine
├── playbooks/
│   ├── playbook_library.py        # 10 pre-built GHL task playbooks
│   └── scheduler.py               # APScheduler — automated playbook execution
├── interfaces/
│   ├── sms_interface.py           # Twilio SMS command interface
│   └── slack_interface.py         # Slack /ghl slash command interface
├── mcp_server/
│   ├── server.py                  # MCP server exposing 11 GHL tools
│   ├── mcp_config.json            # Claude Desktop config
│   └── README.md                  # MCP integration guide
├── ghl_snapshot/
│   ├── snapshot_config.json       # GHL Marketplace product definition
│   └── installer.py               # Auto-configures new client sub-accounts
├── dashboard/
│   └── index.html                 # Mobile-optimized landing page + live dashboard
├── .env.example                   # All environment variables documented
├── requirements.txt               # Python dependencies
├── Dockerfile                     # Multi-stage container build
├── docker-compose.yml             # Full stack with Postgres + Redis
└── README.md
```

---

## Quick Start

### 1. Clone and configure

```bash
git clone https://github.com/YOUR_USERNAME/agentai888.git
cd agentai888
cp .env.example .env
nano .env
```

### 2. Install dependencies

```bash
pip install -r requirements.txt --break-system-packages
playwright install chromium
playwright install-deps chromium
```

### 3. Start the server

```bash
uvicorn server.main:app --host 0.0.0.0 --port 8000
```

### 4. Open the dashboard

```
http://YOUR_SERVER_IP:8000
```

---

## Environment Variables

| Variable | Description |
|---|---|
| `ANTHROPIC_API_KEY` | Claude API key from console.anthropic.com |
| `GHL_API_KEY` | GoHighLevel API key from GHL Settings |
| `GHL_LOCATION_ID` | Your GHL sub-account location ID |
| `GHL_EMAIL` | GHL login email for Browser Agent |
| `GHL_PASSWORD` | GHL login password for Browser Agent |
| `GHL_2FA_SECRET` | TOTP secret if 2FA is enabled (optional) |
| `APP_URL` | Your public server URL |
| `SLACK_WEBHOOK_URL` | Slack webhook for HITL notifications |
| `HITL_APPROVE_URL` | Public URL for HITL approve endpoint |
| `HITL_REJECT_URL` | Public URL for HITL reject endpoint |
| `LEAD_SCORE_THRESHOLD` | Minimum score to trigger outreach (default: 50) |
| `SLA_HOURS` | Hours before SLA escalation (default: 48) |
| `ALLOWED_PHONE_NUMBERS` | Comma-separated numbers allowed to SMS the agent |
| `SLACK_BOT_TOKEN` | Slack bot token for slash commands |

---

## GHL Setup

### Create GHL Workflows

| Workflow Name | Trigger | Webhook URL |
|---|---|---|
| AgentAI888 — New Lead | Contact Created | `POST /webhooks/ghl/lead-created` |
| AgentAI888 — ai-outreach | Tag Added: ai-outreach | `POST /webhooks/ghl/tag-added` |
| AgentAI888 — ai-followup | Tag Added: ai-followup | `POST /webhooks/ghl/tag-added` |
| AgentAI888 — ai-book | Tag Added: ai-book | `POST /webhooks/ghl/tag-added` |
| AgentAI888 — ai-crm | Tag Added: ai-crm | `POST /webhooks/ghl/tag-added` |

Full URL format: `http://YOUR_SERVER_IP:8000/webhooks/ghl/lead-created`

### Add Custom Fields

GHL Settings → Custom Fields → + Add Field

| Field Name | Type |
|---|---|
| Lead Score | Number |
| AI Status | Text |
| Last AI Action | Text |

### Add System Tags

`ai-outreach` · `ai-followup` · `ai-book` · `ai-crm` · `ai-complete` · `needs-review` · `needs-human` · `hot-lead`

---

## Browser Agent

```bash
# Custom task
curl -X POST http://YOUR_SERVER_IP:8000/agents/browser/task \
  -H "Content-Type: application/json" \
  -d '{"task": "Buy a phone number with area code 713", "hitl_mode": "auto"}'

# Build an industry funnel from plain English
curl -X POST http://YOUR_SERVER_IP:8000/agents/browser/funnel \
  -H "Content-Type: application/json" \
  -d '{"command": "roofing leads funnel for storm damage in Houston"}'

# HITL queue
GET  /agents/browser/hitl/queue
POST /agents/browser/hitl/approve/{id}
POST /agents/browser/hitl/reject/{id}
```

---

## Playbooks

| Playbook Key | What It Does | Auto Schedule |
|---|---|---|
| `buy_phone_number` | Purchases a GHL phone number | — |
| `create_funnel` | Builds a lead capture funnel | — |
| `create_funnel_with_phone` | Phone number + funnel in one shot | — |
| `create_lead_workflow` | Builds a full automation workflow | — |
| `create_pipeline` | Creates a new pipeline with stages | — |
| `clean_stale_contacts` | Flags contacts with no activity 30+ days | Sunday 6am |
| `daily_lead_report` | Pulls overnight metrics and summarizes | Daily 7am |
| `weekly_performance_report` | Full week summary | Monday 8am |
| `respond_to_inbox` | Responds to unanswered GHL messages | Every 4 hours |
| `new_client_setup` | Complete sub-account configuration | — |

```bash
curl -X POST http://YOUR_SERVER_IP:8000/api/playbooks/run \
  -H "Content-Type: application/json" \
  -d '{"playbook": "daily_lead_report", "variables": {}}'
```

---

## Interfaces (SMS + Slack)

### SMS via Twilio

Twilio webhook → `POST http://YOUR_SERVER_IP:8000/interfaces/sms`

```
LIST                          — Show all playbooks
RUN daily_lead_report         — Run a named playbook
DO buy a 713 phone number     — Custom GHL task
FUNNEL roofing leads Houston  — Build an industry funnel
STATUS                        — System status
HELP                          — All commands
```

### Slack

Slash command → `POST http://YOUR_SERVER_IP:8000/interfaces/slack`

```
/ghl list
/ghl run daily_lead_report
/ghl do create a pipeline for med spa clients
/ghl funnel dental implant consultation page
/ghl status
```

---

## MCP Server

Add to Claude Desktop config:

```json
{
  "mcpServers": {
    "agentai888-ghl": {
      "command": "python",
      "args": ["/path/to/agentai888/mcp_server/server.py"],
      "env": {
        "AGENTAI_BASE_URL": "https://your-server.com"
      }
    }
  }
}
```

Tools: `ghl_send_sms` · `ghl_score_lead` · `ghl_book_appointment` · `ghl_update_contact` · `ghl_create_funnel` · `ghl_buy_phone_number` · `ghl_create_workflow` · `ghl_browser_task` · `ghl_run_playbook` · `ghl_get_audit_log` · `ghl_system_status`

---

## Deploy to Production

### DigitalOcean (Recommended)

```bash
apt update && apt install -y git python3-pip
git clone https://github.com/YOUR_USERNAME/agentai888.git
cd agentai888
pip install -r requirements.txt --break-system-packages
playwright install chromium && playwright install-deps chromium
cp .env.example .env && nano .env
pip install supervisor --break-system-packages
```

Supervisor config at `/etc/supervisor/conf.d/agentai888.conf`:

```ini
[program:agentai888]
command=uvicorn server.main:app --host 0.0.0.0 --port 8000
directory=/root/agentai888
autostart=true
autorestart=true
stderr_logfile=/var/log/agentai888.err.log
stdout_logfile=/var/log/agentai888.out.log
```

```bash
supervisorctl reread && supervisorctl update && supervisorctl start agentai888
```

### Docker

```bash
docker-compose up -d
```

### Check logs

```bash
tail -f /var/log/agentai888.out.log
```

---

## API Reference

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Dashboard |
| POST | `/webhooks/ghl/lead-created` | New lead webhook |
| POST | `/webhooks/ghl/tag-added` | Tag added webhook |
| POST | `/webhooks/ghl/stage-changed` | Stage changed webhook |
| POST | `/webhooks/deploy` | Deploy agent button |
| POST | `/agents/browser/task` | Assign Browser Agent task |
| POST | `/agents/browser/funnel` | Build industry funnel |
| GET | `/agents/browser/status/{id}` | Task status |
| POST | `/agents/browser/hitl/approve/{id}` | HITL approve |
| POST | `/agents/browser/hitl/reject/{id}` | HITL reject |
| GET | `/agents/browser/hitl/queue` | HITL queue |
| GET | `/api/agents` | All agent statuses |
| GET | `/api/audit` | Audit log |
| GET | `/api/kpi` | KPI + pipeline data |
| GET | `/api/playbooks` | List playbooks |
| POST | `/api/playbooks/run` | Run playbook |
| POST | `/interfaces/sms` | Twilio SMS |
| POST | `/interfaces/slack` | Slack commands |
| GET | `/health` | Health check |

---

## Troubleshooting

| Problem | Fix |
|---|---|
| No webhook received | Check GHL workflow is Published. Verify webhook URL has correct server IP. |
| Server won't start | Check `.env` — verify all keys, no extra spaces. |
| Supervisor shows STOPPED | Run: `supervisorctl start agentai888` |
| Browser Agent login fails | Check `GHL_EMAIL` and `GHL_PASSWORD`. Use a dedicated GHL user. |
| Lead score not in GHL | Verify custom fields created with exact names. |
| Port 8000 unreachable | DigitalOcean → Networking → Firewalls → open inbound port 8000. |
| Duplicate webhooks | Agent Router has a per-contact lock — duplicates dropped automatically. |

---

## License

MIT — The Alleges Group Inc. © 2026

**The Alleges Group Inc.**
AI Workforce Infrastructure × GoHighLevel
*Powered by Claude AI · Playwright · FastAPI · Python*
