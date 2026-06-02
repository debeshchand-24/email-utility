# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This is a Gmail automation utility — no source code exists yet; Claude Code operates directly via the Gmail MCP tools to process emails on command. There is no build, lint, or test step.

## How to operate

Trigger either mode by user command:
- **"run the full inbox cleanup"** → MODE 1 (full unread backlog, ~4000 emails)
- **"run the weekly email cleanup"** → MODE 2 (last 7 days only)

Both modes use the Gmail MCP tools (`mcp__gmail__*`) for all Gmail operations. Process emails in **batches of 20** and report progress after each batch.

## Gmail label taxonomy

All labels follow the `AI/` prefix namespace. Create labels that don't exist before applying them. The special label `AI/Action-Needed` is always combined with a Gmail star.

---

# Email AI Utility

## My identity
- Name: Debesh Chand
- Email: devchand24@gmail.com
- Profession: Product Manager
- Location: Bengaluru, India

## My communication style
- I prefer direct and concise replies
- Professional with colleagues and clients
- Warm and casual with friends and family
- Never overly formal

## Email categories
Apply exactly these Gmail labels (create if they don't exist):
- AI/Work — work, clients, professional
- AI/Bills — invoices, bank, utilities, insurance
- AI/Newsletters — subscriptions, digests, marketing
- AI/Orders — ecommerce, delivery, receipts
- AI/Travel — flights, hotels, cabs, itineraries
- AI/Personal — friends, family, personal
- AI/Spam — unsolicited junk (never delete, just label)
- AI/Other — anything that doesn't fit above

## Priority rules — what needs my attention
Mark as action-needed (apply label AI/Action-Needed and star it) if:
- Someone is directly asking me a question
- A deadline or time-sensitive decision is mentioned
- It's from my manager, key clients, or close colleagues
- There's a payment due, contract to sign, or form to fill
- It's a personal email from family or close friends
- A meeting needs to be confirmed

Do NOT mark as action-needed:
- Newsletters or marketing emails
- Auto-generated order/delivery notifications
- App notifications (LinkedIn, Twitter, etc.)
- Bank statements with no action required

## People I care most about
- Prachi Trivedi — spouse
- Debashis Chanda – parent
- Mitu Chand – parent
- Debeshwar Chand – brother
- Any iiml email ID - IIML friend
- Fresh Chapter - self venture

## Low priority senders
- Any no-reply@ addresses unless they contain an invoice
- LinkedIn notification emails
- Food app promotional emails (Zomato, Swiggy)
- Credit card promotional emails
- Other consumer app promotional emails (Bookmyshow, easemytrip, bigbasket, indigo, goibibo, slack, nobroker and so on) 

## Draft reply rules
1. Match the tone of the original email
2. Keep replies under 100 words unless detail is needed
3. Never make up facts — use [ACTION NEEDED] where I need to fill in
4. Sign off as: Debesh

## Two operating modes

### MODE 1 — One-time full cleanup (run once on command)
When I say "run the full inbox cleanup", do this:

#### Step 1 — Fetch all unread emails
Search Gmail for all unread emails: `is:unread`
Process in batches of 20 to avoid timeouts
This may take multiple rounds — keep going until all are processed

#### Step 2 — Categorise
For each email apply the matching AI/* label
If unsure, use AI/Other and flag it in the report
Mark newsletters, spam, and promotional emails directly —
  no need to flag these for my attention

#### Step 3 — Triage
Apply AI/Action-Needed label and star only emails that
meet the priority rules above
For ~4000 emails, be conservative — only flag genuine
action items, not maybes

#### Step 4 — Bulk archive
After labelling, move all emails that are:
- AI/Newsletters
- AI/Spam
- AI/Orders (older than 30 days)
- AI/Bills (older than 30 days)
...out of the inbox by removing the INBOX label
This cleans up the inbox without deleting anything

#### Step 5 — Draft replies
For every AI/Action-Needed email, create a Gmail draft reply
Use my communication style and draft reply rules above

#### Step 6 — Digest
Send a summary email to devchand24@gmail.com with:
Subject: "📬 Full Inbox Cleanup Report — [date]"
- Total emails processed
- Count per category
- Emails needing my attention (sender, subject, 1-line summary)
- Drafts created and ready to send
- Top 10 senders by volume (unsubscribe candidates)
- Oldest unread email found and its date

#### Step 7 — Report back
Tell me when each batch of 20 is done so I can track progress
Tell me final totals when complete

---

### MODE 2 — Weekly maintenance (runs every week)
When I say "run the weekly email cleanup", do this:

#### Step 1 — Fetch
Search Gmail for all unread emails from the last 7 days:
`is:unread after:[date 7 days ago]`
Process in batches of 20

#### Step 2 — Categorise
For each email apply the matching AI/* label

#### Step 3 — Triage
Apply AI/Action-Needed label and star if it meets priority rules

#### Step 4 — Draft replies
For every AI/Action-Needed email, create a Gmail draft reply

#### Step 5 — Archive
Move newsletters, spam, and old orders/bills out of inbox

#### Step 6 — Digest
Send summary email to devchand24@gmail.com with:
Subject: "📬 Weekly Email Digest — [date]"
- Needs your attention (sender, subject, 1-line summary)
- Drafts ready to send
- What was filed (count per category)
- Suggested unsubscribes (senders with 3+ emails this week)

#### Step 7 — Report
- Total emails processed
- Count per category
- How many action-needed
- How many drafts created
- Anything you were unsure about

---

## Hard rules (apply to BOTH modes)
- NEVER delete any email
- NEVER send any email — only save as draft
- NEVER mark emails as read unless I ask
- Always ask before any irreversible action
- If a batch fails, report the error and continue with the next batch
- When processing 4000+ emails, prioritise completing
  categorisation over perfection — speed matters for the one-time run

