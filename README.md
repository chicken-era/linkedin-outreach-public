# linkedin-outreach-public

A Claude skill for customer discovery. You describe what you're trying to learn and who you want to talk to. Claude then:

1. Finds relevant people on LinkedIn, by company and job title.
2. Tracks them with Contact Name, Company / Role, Segment, Found By, Stage, LinkedIn URL, and a tailored message. You can keep this in a downloadable spreadsheet or in an app you already use, like Notion, Google Sheets, Airtable, or a CRM.
3. Sends them connection requests without a note, from your own LinkedIn account.
4. Writes a tailored follow-up message for each person.
5. Later, when you ask, checks who accepted and sends them their message, but only after you approve the exact messages. With LinkedIn Premium it can also send InMails or invite notes, again only with your approval.

## Before you start

You need:
- A Claude plan that supports skills.
- One browser Claude can drive:
  - **Claude in Chrome**: the Chrome extension, connected to your Claude account. It uses your existing LinkedIn sign-in in Chrome.
  - **Claude desktop app**: its built-in browser pane. Sign in to LinkedIn there once. *(Should work; less tested than Chrome so far.)*
- To be signed in to LinkedIn in that browser.
- Optional: your tracking app (Notion, Google Sheets, Airtable, HubSpot...) connected to Claude as a connector, if you want contacts kept there.

Help: https://support.claude.com (search "Claude in Chrome", "skills", or "connectors").

## Install

Download this repo (green **Code** button → **Download ZIP**) and add the `linkedin-outreach-public` folder (it contains `SKILL.md`) as a skill in Claude.

## Use

Open a new chat and say something like:

> Use the linkedin-outreach-public skill to find 30 people for customer discovery.

Claude first checks which browser it can use, and that you're signed in to LinkedIn. Then it asks for:
- your name and affiliation
- what you want to learn
- your target markets and job titles
- how many contacts you want
- your outreach message (or it drafts one from a proven template and shows you an example first)
- where to track contacts

Later, to follow up:

> Check who accepted and send them their messages.

## Caveats: read before using

- **Your account, your risk.** LinkedIn's User Agreement restricts automated activity. Even at low volume, LinkedIn can warn, restrict, or temporarily block an account it thinks is automated. Use it sparingly.
- **Invite limits.** LinkedIn doesn't publish them. People commonly see about **100 invites per rolling 7 days**, and Premium doesn't raise that. The skill defaults to **20 a day** (5 for new or rarely used accounts), counts what you've already sent this week, and splits bigger lists across days.
- **Free vs. Premium.** Free accounts get only a few personalized invite notes a month, capped at about 300 characters. Premium adds InMail credits and more notes, but not more invites.
- **Messages are never sent without your OK.** It shows you each message and who it goes to, and sends only what you approve. It never replies to replies for you.

### How not to get blocked
- Keep daily volume low and steady. Don't send a big batch in one sitting.
- Only invite people who clearly match. A low acceptance rate is one of the biggest red flags.
- Withdraw old pending invites now and then (My Network → Manage → Sent).
- If LinkedIn shows any warning, CAPTCHA, or identity check, stop for a few days. The skill stops automatically and never tries to get around them.
- New or rarely used accounts: start slow (about 5 a day) for the first couple of weeks.

## Responsible use

Contact only people relevant to a genuine discovery purpose, and never use this for spam or mass selling.
