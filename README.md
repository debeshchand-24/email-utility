# Gmail AI Utility

A fully automated Gmail management system that categorises, triages, drafts replies, and delivers a weekly digest. Built using Claude Routines and Gmail MCP. Zero application code written.

**Status:** Production. Running every Saturday 9am IST via Claude Routines.

---

## The problem

4000+ unread emails, no monitoring habit, cumbersome manual cleanup. Needed a system that runs itself, surfaces only what matters, and prepares responses without touching the inbox manually. Goal: spend 15 minutes per week on email, not 2 hours.

---

## What it does

- Categorises all incoming emails into 8 labels: Work, Bills, Newsletters, Orders, Travel, Personal, Spam, Other
- Triages action-needed emails and stars them based on sender, content, and urgency rules
- Drafts replies automatically for every flagged email, matching tone and context
- Archives newsletters and spam out of inbox automatically
- Sends a weekly digest summarising what needs attention and what was filed
- Runs automatically every Saturday 9am IST with no laptop required

---

## Architecture

Claude Routine fires Saturday 9am IST
Claude Sonnet reads CLAUDE.md instructions
Gmail MCP fetches unread emails from last 7 days
Process in batches of 20:
  Categorise  - Apply AI/* labels
  Triage      - Star + AI/Action-Needed label
  Archive     - Remove INBOX label for newsletters and spam
  Draft       - Create Gmail draft replies
Weekly digest draft sent to inbox

---

## Stack

| Component | Tool |
|-----------|------|
| AI engine | Claude Sonnet 4.6 via Claude Routines |
| Gmail integration | GongRzhe Gmail MCP server |
| Scheduling | Claude Routines - cloud-based, no local machine needed |
| Auth | Google OAuth 2.0 |
| Config | CLAUDE.md - natural language instruction file |

---

## Key design decisions

**1. CLAUDE.md as the brain**
All logic lives in a natural language instruction file, not code. Changing behaviour means editing a markdown file. No Python knowledge needed to update rules. The instruction file is simultaneously the product spec and the implementation.

**2. Cloud scheduling via Routines, not cron**
Cron requires the laptop to be on. Claude Routines run on Anthropic infrastructure - more reliable, has run history, zero dependency on local machine state.

**3. Batching in groups of 20**
Manages Gmail API rate limits and context window constraints. Partial failures do not restart the whole run. Reliable even at high email volumes.

**4. Draft-only, never send**
Hard rule in CLAUDE.md: Claude never sends an email, only saves drafts. Human stays in the loop for all outbound communication. The cost of a wrong send is higher than the cost of one extra click.

**5. MCP over direct API**
Using Gmail MCP instead of writing Gmail API code reduced setup from ~200 lines of Python to zero application code. Plumbing handled entirely by the MCP layer.

**6. Natural language rules over code**
Priority rules, tone matching, and categorisation logic are all in plain English. Easier to iterate, easier to audit, easier to hand off.

---

## What is imperfect

No feedback loop. When a draft reply is wrong in tone or a categorisation is off, there is no mechanism to log that and improve the rules systematically. Current fix cycle: notice the error manually, update CLAUDE.md, verify on next run. A structured eval dataset with labelled emails would make this loop faster and more reliable.

---

## What I would do differently

Build a 30-email labelled test dataset before writing the CLAUDE.md rules. Right now there is no precision or recall number for categorisation accuracy. I know it works well from observation but cannot prove it with a number. That matters when the system is making decisions on your behalf every week.

---

## Roadmap

- Eval suite with 30-email test dataset to measure categorisation accuracy and action-needed precision
- Version control discipline on CLAUDE.md to track prompt iterations and enable rollback
- Phase 2: one-time full cleanup of 4000+ backlog emails
- Expand to a second email account
