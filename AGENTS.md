# Agents — SocialSculp

Agents available in **Claude Code** (`.claude/commands/`) and **Cursor** (`.cursor/skills/`).

---

## Email Outreach Agent

**Purpose:** Draft and refine cold emails, follow-ups, and B2B sales outreach for SocialSculp × SylcRoad — creator seeding, partnerships, and brand deals.

**Where it lives:** `.cursor/skills/email-outreach/`

**How to use it:**
1. Open **Agent** chat (Cmd+L or Ctrl+L).
2. Type **`/email-outreach`** in the input.
3. Add your request — e.g. *"Draft a cold email to [Brand] about creator seeding"* or *"Write a follow-up for my Cal AI outreach"*.
4. Send.

The agent only runs when you invoke it with `/email-outreach`. It won’t apply automatically to other work.

**What it knows:**
- SocialSculp × SylcRoad context (creator seeding, packages, case studies)
- Cold email, follow-up, and partnership templates
- Tone, subject lines, and personalization checklist
- Examples in `.cursor/skills/email-outreach/examples.md`

---

## Communications & Outreach Agent (Psychology)

**Purpose:** 160 IQ sales and outreach psychologist. Covers cold email, warm network reactivation, Instagram DMs, LinkedIn, SMS/WhatsApp, and follow-up sequences. Audits drafts for intent leakage, friction, and conversion issues. Applies Cialdini principles, curiosity gaps, pattern interrupts, and channel-specific psychology.

**Where it lives:** `.claude/commands/comms-agent.md` (Claude Code)

**How to use it:**
1. Open Claude Code in this project.
2. Type **`/comms-agent`** followed by your request.
3. Examples:
   - `/comms-agent Audit this message: [paste draft]`
   - `/comms-agent Write a warm reconnect to Lucas — haven't spoken in 4 months`
   - `/comms-agent Cold IG DM to a DTC fitness brand`
   - `/comms-agent Follow-up #2 for my Cal AI email`
   - `/comms-agent Best approach to re-engage a warm lead who ghosted?`

**What it knows:**
- Cold vs warm vs hot approach psychology
- Channel rules: email, IG DM, LinkedIn, SMS/WhatsApp
- Follow-up timing and sequencing (including break-up email)
- Cialdini's 6 principles applied to outreach
- Intent leakage detection and draft auditing
- SocialSculp × SylcRoad case studies and packages

---

## Website Designer Agent

**Purpose:** Scrape any website at DevTools depth (HTML, computed CSS, fonts, animations, CSS variables, network, DOM tree, screenshot) and synthesize new Astro/Next.js components from the extracted design system. First target: mont-fort.com → S23 Operations site.

**Where it lives:** `.claude/commands/website-designer.md` (Claude Code)

**How to use it:**
1. Open Claude Code in this project.
2. Type **`/website-designer`** followed by your request.
3. Examples:
   - `/website-designer Scrape https://example.com and give me the design system`
   - `/website-designer Generate a hero section for the S23 site based on mont-fort`
   - `/website-designer Build the contact section`
   - `/website-designer Analyze the mont-fort scraper output`

**What it knows:**
- Playwright scraper at `scraper/` — runs `npx tsx scrape.ts <url>`
- S23 Operations site at `s23-operations/` — Astro 5 + Tailwind v4
- mont-fort.com design system (scraped reference)
- Design principles: dark minimal, Space Grotesk, #c0c6d6 accent, 0.4s linear transitions
- S23 Operations content (divisions, partners, case study stats)

---

## Quick reference

| Agent            | Invoke with          | Use for                              |
|------------------|----------------------|--------------------------------------|
| Email Outreach   | `/email-outreach`    | Cold emails, follow-ups, partnerships |
| Comms & Outreach | `/comms-agent`       | All channels, psychology, draft audits, IG DMs, warm network |
| Website Designer | `/website-designer`  | Scrape sites, extract design systems, build Astro components |
