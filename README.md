# linkedin-job-search-outreach

*(This is the `job-search` branch. For customer discovery, use the `main` branch.)*

A Claude skill for job hunting on LinkedIn. You tell it the roles and companies you're after. Claude then:

1. Finds the right people at each company: people already in the role, likely hiring managers, recruiters, and alumni from your school (who reply the most).
2. Tracks them as a job-search pipeline (Contact, Company / Role, Contact Type, Relationship, Target Job, Stage, LinkedIn, and a tailored message) in a spreadsheet or an app like Notion, Google Sheets, or Airtable.
3. Sends connection requests without a note, from your own LinkedIn account.
4. Writes a short, tailored message for each person, asking for a 15-minute informational chat. Hiring managers and recruiters get their own templates.
5. Later, when you ask, checks who accepted and sends their messages, but only after you approve them. After a chat, it can draft a thank-you note or a referral ask for a specific posting, again only with your OK.

It never applies to jobs, sends email, or replies to people for you.

## Before you start

You need:
- A Claude plan that supports skills.
- One browser Claude can drive:
  - **Claude in Chrome**: the Chrome extension, connected to your Claude account. It uses your existing LinkedIn sign-in in Chrome.
  - **Claude desktop app**: its built-in browser pane. Sign in to LinkedIn there once. *(Should work; less tested than Chrome so far.)*
- To be signed in to LinkedIn in that browser.
- Optional: your tracking app (Notion, Google Sheets, Airtable...) connected to Claude, if you want your pipeline there.

Tip: update your LinkedIn headline and About section to match the roles you're targeting. People check your profile before accepting.

Help: https://support.claude.com (search "Claude in Chrome", "skills", or "connectors").

## Install

On GitHub, switch to the **job-search** branch, then click the green **Code** button → **Download ZIP**. Add the downloaded folder (it contains `SKILL.md`) as a skill in Claude.

Or tell your Claude:

> Add the skill from the job-search branch of https://github.com/chicken-era/linkedin-outreach-public

## Use

Open a new chat and say something like:

> Use the linkedin-job-search-outreach skill to find 20 people at [companies] for [role] roles.

Claude first checks your browser and LinkedIn sign-in. Then it asks for:
- your name and a one-line background
- your target roles and companies
- which kinds of people to reach (people in the role, hiring managers, recruiters, alumni)
- any job posting links
- your message (or it drafts one and shows you an example first)
- where to track your pipeline

Later:

> Check who accepted and send them their messages.

> I had a great chat with Ana at Acme. Draft a thank-you and ask about a referral for this posting: [link]

## Caveats: read before using

- **Your account, your risk.** LinkedIn's User Agreement restricts automated activity. Even at low volume, LinkedIn can warn, restrict, or temporarily block an account it thinks is automated, and a restricted profile mid-job-search hurts. Use it sparingly.
- **Invite limits.** LinkedIn doesn't publish them. People commonly see about **100 invites per rolling 7 days**, and Premium doesn't raise that. The skill defaults to **20 a day** (5 for new or rarely used accounts), counts what you've already sent this week, and splits bigger lists across days.
- **Free vs. Premium.** Free accounts get only a few personalized invite notes a month, capped at about 300 characters. Premium adds InMail credits (useful for recruiters) and more notes, but not more invites.
- **Messages are never sent without your OK.** It shows you each message and who it goes to, and sends only what you approve.
- **Fewer, better contacts win.** Recruiters and hiring managers get flooded. Alumni and people in the role are your best bet.

### How not to get blocked
- Keep daily volume low and steady. Don't send a big batch in one sitting.
- Only invite people who clearly match. A low acceptance rate is one of the biggest red flags.
- Withdraw old pending invites now and then (My Network → Manage → Sent).
- If LinkedIn shows any warning, CAPTCHA, or identity check, stop for a few days. The skill stops automatically and never tries to get around them.
- New or rarely used accounts: start slow (about 5 a day) for the first couple of weeks.

## Responsible use

Contact only people relevant to your real job search, and never use this for spam.
