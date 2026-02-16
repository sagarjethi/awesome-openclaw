# OpenClaw Use Cases

Detailed, implementation-ready use cases built and verified by the community. Each one includes the skills needed, setup instructions, and real configuration examples.

> Inspired by [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases) — a community collection of real-world OpenClaw implementations.

**[← Back to main list](README.md)**

---

## Contents

- [1. Self-Healing Home Server](#1-self-healing-home-server)
- [2. Multi-Agent Team for Solo Founders](#2-multi-agent-team-for-solo-founders)
- [3. Family Calendar & Household Assistant](#3-family-calendar--household-assistant)
- [4. Daily Reddit Digest](#4-daily-reddit-digest)
- [5. YouTube Content Pipeline](#5-youtube-content-pipeline)
- [6. Personal Knowledge Base (RAG)](#6-personal-knowledge-base-rag)
- [7. X (Twitter) Account Analysis](#7-x-twitter-account-analysis)
- [8. Goal-Driven Autonomous Tasks (Overnight Mini-App Builder)](#8-goal-driven-autonomous-tasks-overnight-mini-app-builder)
- [9. Polymarket Autopilot (Paper Trading)](#9-polymarket-autopilot-paper-trading)

---

## 1. Self-Healing Home Server

> **Category:** Infrastructure & DevOps · **Complexity:** Advanced · **Source:** [Nathan's Deep Dive](https://madebynathan.com/2026/02/03/everything-ive-done-with-openclaw-so-far/)

Turns OpenClaw into a persistent infrastructure agent that monitors, diagnoses, and fixes server issues before you know there's a problem.

**What it does:**
- Continuous health monitoring of services, containers, and system resources
- Autonomous remediation (pod restarts, resource scaling, config repairs)
- Infrastructure provisioning via Terraform, Ansible, Kubernetes
- Daily briefings (system status, weather, calendar, priorities)
- Automated security audits (exposed secrets, privilege escalation, excessive permissions)

**Required access:** SSH to home network, kubectl, Terraform/Ansible, 1Password CLI, email CLI, calendar, Obsidian vault

<details>
<summary><b>AGENTS.md Configuration</b></summary>

```markdown
## Infrastructure Agent

You are Reef, an infrastructure management agent.

Access:
- SSH to all machines on the home network (192.168.1.0/24)
- kubectl for the K3s cluster
- 1Password vault (read-only for credentials, dedicated AI vault)
- Gmail via gog CLI
- Calendar (yours + partner's)
- Obsidian vault at ~/Documents/Obsidian/

Rules:
- NEVER hardcode secrets — always use 1Password CLI or environment variables
- NEVER push directly to main — always create a PR
- Run `openclaw doctor` as part of self-health checks
- Log all infrastructure changes to ~/logs/infra-changes.md
```
</details>

<details>
<summary><b>HEARTBEAT.md Schedule</b></summary>

```markdown
Every 15 minutes:
- Check kanban board for in-progress tasks → continue work

Every hour:
- Monitor health checks (Gatus, ArgoCD, service endpoints)
- Triage Gmail (label actionable items, archive noise)

Every 6 hours:
- Knowledge base data entry (process new Obsidian notes)
- Self health check (openclaw doctor, disk usage, memory, logs)

Daily:
- 4:00 AM: Nightly brainstorm (explore connections between notes)
- 8:00 AM: Morning briefing (weather, calendars, system stats, task board)

Weekly:
- Knowledge base QA review
- Infrastructure security audit
```
</details>

<details>
<summary><b>Security Checklist (mandatory before granting SSH)</b></summary>

```markdown
1. Pre-push hooks:
   - Install TruffleHog secret scanner on ALL repositories
   - Block any commit containing hardcoded API keys or tokens

2. Local-first Git workflow:
   - Use Gitea (self-hosted) for private code before pushing to public GitHub
   - CI scanning pipeline runs before any public push
   - Human review required before main branch merges

3. Defense in depth:
   - Dedicated 1Password vault for AI agent (limited scope)
   - Network segmentation for sensitive services
   - Daily automated security audits

4. Agent constraints:
   - Branch protection: PR required for main, agent cannot override
   - Read-only access where write isn't needed
   - All changes logged and auditable via git
```
</details>

**Key takeaway:** "AI assistants will happily hardcode secrets. Secret scanning and pre-commit hooks are non-negotiable."

---

## 2. Multi-Agent Team for Solo Founders

> **Category:** Business & Productivity · **Complexity:** Advanced · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/multi-agent-team.md)

One agent can't do everything well — context windows fill up fast when juggling multiple domains. This sets up a coordinated team of specialized agents controlled from a single Telegram group.

**Agent Roster:**

| Agent | Role | Model | Daily Schedule |
|-------|------|-------|----------------|
| **Milo** | Strategy lead & team coordinator | Claude Opus | Morning standup at 8 AM |
| **Josh** | Business & growth analytics | Claude Sonnet | Metrics pull at 9 AM |
| **Marketing** | Web research & trend analysis | Gemini | Content ideas at 10 AM |
| **Dev** | Coding & technical decisions | Claude Opus | On-demand |

**How it works:**
- Single Telegram group routes messages via tagging (`@milo`, `@josh`, `@marketing`, `@dev`)
- Milo is the default handler for untagged messages
- Shared memory via `GOALS.md`, `DECISIONS.md`, `PROJECT_STATUS.md`
- Each agent has private directories for domain-specific notes

<details>
<summary><b>Shared Memory Structure</b></summary>

```
~/openclaw-team/
├── shared/
│   ├── GOALS.md              # Company objectives (all agents read)
│   ├── DECISIONS.md          # Decision log (all agents append)
│   └── PROJECT_STATUS.md     # Current sprint status
├── milo/                     # Strategy notes, meeting summaries
├── josh/                     # Analytics data, growth experiments
├── marketing/                # Campaign ideas, competitor research
└── dev/                      # Technical specs, architecture notes
```
</details>

**Key takeaway:** "Personality matters more than you'd think." Start with 2 agents before expanding. Shared memory + private context lets agents maintain both collective alignment and individual expertise.

---

## 3. Family Calendar & Household Assistant

> **Category:** Home & Life · **Complexity:** Medium · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/family-calendar-household-assistant.md)

Modern families manage 5+ calendars across different platforms. This aggregates them into a unified system with ambient monitoring and household coordination.

**What it does:**
- **Morning briefing** — Consolidated daily summary from all family calendars with 3-day lookahead conflict detection
- **Ambient message monitoring** — Detects appointment confirmations in iMessages, auto-creates calendar events
- **Travel buffers** — Adds 30-minute driving blocks before/after appointments
- **Inventory tracking** — Pantry/fridge inventory queryable via Telegram with photo-based OCR input
- **Grocery deduplication** — Tracks low-stock items, generates combined shopping lists

**Required skills:** Google Calendar API, Apple Calendar (iCal), iMessage monitoring (macOS), Telegram, file system access, camera/OCR processing

<details>
<summary><b>How Ambient Monitoring Works</b></summary>

```markdown
Message monitoring runs every 15 minutes:
1. Scans iMessages for patterns like "appointment confirmed for" or meeting proposals
2. Auto-creates calendar events with driving time buffers
3. Sends family notification to shared Telegram channel

Inventory management:
- Data stored in ~/household/inventory.json
- Partners can update via photo (receipt/shelf scan), text, or voice
- Query: "Do we have milk?" → checks inventory, suggests shopping list if low
```
</details>

**Pro tip:** Run on a dedicated home Mac Mini for best iMessage integration and always-on availability. Start with read-only calendar access before enabling write actions.

---

## 4. Daily Reddit Digest

> **Category:** Social Media & Content · **Complexity:** Easy · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/daily-reddit-digest.md)

Automated daily digest of top-performing posts from your favorite subreddits, personalized by learning your preferences over time.

**Required skill:** [`reddit-readonly`](https://clawhub.ai/buksan1950/reddit-readonly) (no authentication needed)

```bash
openclaw skills install reddit-readonly
```

<details>
<summary><b>Setup Prompt</b></summary>

```markdown
Configure OpenClaw with:
1. Fetch top posts from: r/MachineLearning, r/LocalLLaMA, r/selfhosted (customize)
2. Create a memory system documenting my preferred post types
3. After each digest, ask me 1-2 questions about what I liked/disliked
4. Store my preferences as rules (e.g., "skip meme posts", "prioritize tutorials")
5. Schedule delivery: 5 PM daily via Telegram

The system learns over time — after a week, digests become highly personalized.
```
</details>

**Key feature:** Read-only. No posting, voting, or commenting — just curated reading.

---

## 5. YouTube Content Pipeline

> **Category:** Content Creation · **Complexity:** Medium · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/youtube-content-pipeline.md)

Automates video idea scouting for YouTube creators by scanning news and social media, while preventing duplicate coverage through semantic deduplication.

**What it does:**
- Hourly cron job scans breaking news (web + X/Twitter) and pitches video ideas to Telegram
- Maintains a 90-day video catalog in SQLite with vector embeddings
- Checks against your existing videos to avoid re-covering topics
- Auto-generates Asana/Todoist cards with research outlines

**Required skills:** Web search (built-in), `x-research-v2`, knowledge-base skill, Asana/Todoist, YouTube Analytics via gog CLI, Telegram

<details>
<summary><b>Database Schema</b></summary>

```sql
CREATE TABLE pitches (
    id INTEGER PRIMARY KEY,
    ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    topic TEXT NOT NULL,
    embedding BLOB,         -- vector for semantic dedup
    sources TEXT,            -- JSON array of source URLs
    status TEXT DEFAULT 'pending'  -- pending/approved/rejected
);
```
</details>

**How dedup works:** When a new idea is pitched, its embedding is compared against the last 90 days of pitches and your YouTube Analytics history. If similarity exceeds the threshold, it's flagged as "already covered."

---

## 6. Personal Knowledge Base (RAG)

> **Category:** Research & Learning · **Complexity:** Easy · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/knowledge-base-rag.md)

Drop any URL into Telegram and it auto-ingests the content (articles, tweets, YouTube transcripts, PDFs) into a searchable knowledge base.

**Required skills:** `knowledge-base` (from ClawHub), built-in `web_fetch`

```bash
openclaw skills install knowledge-base
```

**Setup:**
1. Create a "knowledge-base" Telegram topic or Slack channel
2. Configure OpenClaw to auto-fetch any URL dropped in that channel
3. Content is chunked, embedded, and stored with metadata (title, URL, date, type)
4. Query with natural language: *"What did I save about agent memory?"*

**Cross-workflow integration:** Your knowledge base feeds into other automations — e.g., the YouTube pipeline queries it during video research, your morning brief pulls from recently saved articles.

---

## 7. X (Twitter) Account Analysis

> **Category:** Social Media · **Complexity:** Easy · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/x-account-analysis.md)

Free alternative to paid analytics services ($10-$50/mo). OpenClaw fetches your recent tweets and provides qualitative content analysis.

**Required skill:** `bird` (X/Twitter access)

```bash
clawhub install bird
```

**Setup:**
1. Create a dedicated OpenClaw X account (for security)
2. Log into x.com, extract cookie credentials (`auth-token`, `ct0`)
3. Ask OpenClaw to retrieve your last N tweets and analyze patterns

**What you can ask:**
- "What makes my posts go viral vs. flop?"
- "Why is my engagement inconsistent?"
- "Write me a script to analyze my posting schedule vs. engagement"
- "What topics get the most replies vs. likes?"

---

## 8. Goal-Driven Autonomous Tasks (Overnight Mini-App Builder)

> **Category:** Productivity & Creative · **Complexity:** Medium · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/overnight-mini-app-builder.md)

The agent autonomously generates 4-5 daily tasks aligned with your goals — then executes them independently, including building surprise mini-apps overnight.

**How it works:**
1. **Brain dump** — Give OpenClaw detailed context about your career, personal, and business goals
2. **Daily generation** — Every morning, the agent creates actionable tasks advancing those goals
3. **Autonomous execution** — Tasks are completed and tracked on a Kanban dashboard (Next.js)

**Task scope goes beyond code:** Research, content creation, competitive analysis, feature development, MVP applications — anything that advances your goals.

<details>
<summary><b>Example Kanban Dashboard</b></summary>

```
┌──────────────┬──────────────┬──────────────┐
│   TO DO      │ IN PROGRESS  │    DONE      │
├──────────────┼──────────────┼──────────────┤
│ Research     │ Building     │ Published    │
│ competitor   │ landing page │ blog post on │
│ pricing      │ MVP for      │ market       │
│ models       │ feature X    │ analysis     │
│              │              │              │
│ Draft email  │              │ Set up       │
│ to potential │              │ analytics    │
│ partners     │              │ dashboard    │
└──────────────┴──────────────┴──────────────┘
```
</details>

**Key takeaway:** "The more context you give about your goals, the better the daily tasks will be." Most people struggle converting big-picture visions into daily executable steps — this automates both planning and implementation.

---

## 9. Polymarket Autopilot (Paper Trading)

> **Category:** Finance & Trading · **Complexity:** Advanced · **Source:** [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/polymarket-autopilot.md)

Automated simulated trading on prediction markets with backtesting, strategy analysis, and daily performance reports via Discord.

**Strategies:**

| Strategy | Logic | Entry Signal |
|----------|-------|--------------|
| **TAIL** (Trend) | Ride strong trends | >60% probability + volume spike |
| **BONDING** (Contrarian) | Buy sudden drops | Price drops >10% on news |
| **SPREAD** (Arbitrage) | Exploit pricing errors | YES + NO > 1.05 |

**How it runs:**
- Cron job every 15 minutes scans Polymarket for opportunities
- Paper trades logged in PostgreSQL/SQLite with full P&L tracking
- Morning Discord summary: yesterday's trades, portfolio value, win rates, market insights
- Starts with $10,000 simulated capital — **never accesses real funds**

<details>
<summary><b>Database Schema</b></summary>

```sql
CREATE TABLE paper_trades (
    id SERIAL PRIMARY KEY,
    market TEXT NOT NULL,
    strategy TEXT NOT NULL,      -- TAIL, BONDING, or SPREAD
    entry_price DECIMAL(10,4),
    exit_price DECIMAL(10,4),
    pnl DECIMAL(10,4),
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE portfolio (
    id SERIAL PRIMARY KEY,
    total_value DECIMAL(10,2),
    cash DECIMAL(10,2),
    positions JSONB,
    snapshot_at TIMESTAMP DEFAULT NOW()
);
```
</details>

---

> **More use cases:** See the full collection at [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases) — community contributions welcome.

**[← Back to main list](README.md)**
